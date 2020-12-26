---
title:  "Flatbuffers in Unity for ~40x gain"
date:   2020-12-26 14:00:00
categories: [gamedev,unity,programming,rust]
draft: false
---

# Problem

We recently switched from downloading and parsing JSON in our Unity client to a binary format based on Flatbuffers.
In this this article you are going to learn:

* *why* we did that
* *what* Flatbuffers is anyway
* *how* you can do that yourself
* what *benefit* we gained

**TL;DR**

You are looking to simplify your life integrating Flatbuffers in Unity? Look no further: [gameroasters/flatbuffers-unity](https://github.com/gameroasters/flatbuffers-unity-docker)

# Context

Our recent game *Wheelie Royale* ([Appstore](https://apps.apple.com/US/app/id1518264893) / [Playstore](https://play.google.com/store/apps/details?id=com.gameroasters.wheelieroyale&hl=en&gl=US)) downloads a lot of replay data by other players. Replay data is stored in JSON format. In the most extreme cases up to 15mb of JSON for a single level.
While this is already a problem just because of mobile data usage it manifests even more severly in mid- to low-end devices when deserializing the JSON data.

After digging out my low-end test device (Galaxy S4) it took [Newtonsoft.JSON](https://www.newtonsoft.com/json) **20 sec** to deserialize the 15mb. This is bad and for some players it even went up to a minute - an absolute dealbreaker.

Obviously we had to find a better method entirely.

# Flatbuffers

> FlatBuffers is an efficient cross platform serialization library ([Flatbuffers website](https://google.github.io/flatbuffers/))

Initially a Google internal project for game development it received some fame when Facebook announced massive performance gains by utilizing it on their mobile app ([article](https://engineering.fb.com/2015/07/31/android/improving-facebook-s-performance-on-android-with-flatbuffers/)).

Using Flatbuffers gives us two main advantages:

1. Data is stored in binary form making it easy on the bandwidth
2. Accessing Data is very fast since it is just a lookup from contiguous memory

The following graphic visualizes this:

![fb]({{ site.url }}/assets/flatbuffers/fb.jpeg)

Flatbuffers store data in a **contiguous** chunk of memory making it also easy on the garbage collector that especially in our use case was trashed with a lot of small allocations.
If you mainly read your data from a buffer and do not need to alter it (our exact use case) it escentially reduces allocations to zero (for reusing a static buffer).

Aside the compact memory layout Flatbuffers reduces the memory consumption by expecting both parties to know the schema. We later see how we generate code for client and backend to be able to speak the same *language*.

## Comparison

- **Before**: Deserializing 15mb of Json in **20 secs**
- **After**: Parsing the same data but using Flatbuffers (4mb) in **0,5 sec**

This is a speed improvement of **40x**.

**Disclaimer**: Of course this is not a proper scientific benchmarking method but it holds true even on our modern iPhones (albeit on a much smaller scale). I leave the more scientific methods of benchmarking to people smarter than me: 
[benchmark](https://google.github.io/flatbuffers/flatbuffers_benchmarks.html)

# Flatbuffers Schema

Here is a simplified version of our schema file. Keep in mind that we are dealing with playbacks (ghosts) of other players. Each ghost consists of a TON of deltas (Sample) that allow us to replay them.

```fbs
struct Sample {
  //...
  r: int16;
}

table GhostRecording {
  //...
  deltas: [Sample] (required);
}

table Ghost {
  //...
  recording: [GhostRecording] (required);
}

table Ghosts {
  //...
  items:[Ghost] (required);
}

root_type Ghosts;
```

Here you can see why our use case was particularly tough for the garbage collector in unity since we are dealing with a lot of small individually allocated objects.

## Code generation

When it comes to the pipeline required to get this working I was disappointed at how little was available: no easy docker container to get `flatc` (the schema transpiler) working cross platform, no ready built .net library for unity to get started. 

So I built this and open sourced it on our company github: [gameroasters/flatbuffers-unity](https://github.com/gameroasters/flatbuffers-unity-docker)

Using this docker container it is easy to create your serialization/deserialization code using the following snippet:

```sh
docker run -it -v $(shell pwd):/fb gameroasters/flatbuffers-unity:v1.12.0 /bin/bash -c "cd /fb && \
	flatc -n --gen-onefile schema.fbs && \
	flatc -r --gen-onefile schema.fbs"
```

This will mount your current working directory into the container, it is expecting to find a `schema.fbs` file in here and generate the necessary code for *Rust* and *CSharp* for you in two file called `schema.rs` and `schema.cs`.

# Shortcomings

Flatbuffers is not going to make your code more readable. Here is a little snippet how we read in stuff from it:

```cs
var fb_ghosts = GR.WR.Schema.Ghosts.GetRootAsGhosts(new ByteBuffer(data));

var res = new List<Ghost>(fb_ghosts.ItemsLength);
for (var i = 0; i < fb_ghosts.ItemsLength; i++)
{
    var e = fb_ghosts.Items(i);
    if (!e.HasValue) continue;
    var recording = e.Value.Recording.Value;

    var deltas = new List<Sample>(recording.DeltasLength);
    for (var j = 0; j < recording.DeltasLength; j++)
    {
        var delta = recording.Deltas(j);
        var r = delta.Value.R;
        deltas.Add(new Sample(r));
    }

    var ghost = new Ghost();
    ghost.recording = new GhostRecording(); 
    ghost.recording.deltas = deltas;
  
    res.Add(ghost);
}
```

Unlike some alternatives to Flatbuffers it does not create the Plain-Old-Data (POD) objects for you and deserialize into those. But that is by design because you could actually in such cases where you only need to read data go without them. 

We only create those to keep the code compatible with the previous approach that deserialized JSON into POD objects.

# Alternatives

Of course there are alternatives that I do not want to hide:

* [protobuf](https://developers.google.com/protocol-buffers)
* [cap'n'proto](https://github.com/capnproto/capnproto)

Here is a very nice comparison matrix: https://capnproto.org/news/2014-06-17-capnproto-flatbuffers-sbe.html

The main benefit for **protobuf** is that it does the extra step of creating POD objects for you making it even closer to what you are used to in regular JSON deserialization. It is a good tradeoff between the speed (Flatbuffers) and convenience (JSON). Another nice thing is: protobuf also speaks JSON which simplifies debugging a lot.

The other alternative is actually made by the [same guy](https://stackoverflow.com/a/25370932/1397367) as protobuf and uses the same zero-allocation approach Flatbuffers use but does not support as many languages yet - thats the only reason I did not consider giving **cap'n'proto** a try.

Ultimately there is no *best* solution, everything comes at a cost. If you need raw speed there is not much more you can get compared to Flatbuffers.

# Further Resources

* [Bringing FlatBuffers Zero-Copy Serialization to Rust by Robert Winslow](https://www.youtube.com/watch?v=YsiQDX20lXI) (Meetup Video)