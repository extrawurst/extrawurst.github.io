---
title:  "Flatbuffers in Unity for ~14x gain"
date:   2020-12-15 12:00:00
categories: [gamedev,unity,programming,rust]
---

# Problem

We recently switched from downloading and parsing JSON in out Unity client to a binary format based on flatbuffers.
In this this article you are going to learn:

* *why* we did that
* *what* flatbuffers is anyway
* *how* you can do that yourself
* what *benefit* we gained

# Context

Our recent game downloads a lot of replay data by players. Replay data is stored in JSON format. In the most extreme cases up to 15mb of JSON for a single level.
While this is already a problem just because of download bandwidth it manifest even more severly in mid- to low-end devices when deserializing the JSON.

After digging out my low-end test device (Galaxy S4) it took [Newtonsoft.JSON TODO](TODO) **20 sec** to deserialize the 15mb. This is bad and for some players it even went up to a minute - an absoulte dealbreaker.

Obviously we had to find a better method entirely.

# Flatbuffers


## Outcome

Before: Deserializing 15mb of Json in 20s
After: parsing the same data but using flatbuffers (4mb) in 1,5s

# Flatbuffers Schema

```fbs
struct Pos {
  x: int8;
  y: int8;
}

struct Sample {
  p: Pos;
  r: int16;
}

table Ghost {
  deltas: [Sample] (required);
}

table Ghosts {
  items:[Ghost] (required);
}

root_type Ghosts;
```

## Code generation

```Dockerfile
# docker container for building flatc binary
FROM ubuntu
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y g++ git cmake make
RUN git clone --branch v1.12.0 --depth 1 https://github.com/google/flatbuffers.git
RUN cd flatbuffers && cmake -G "Unix Makefiles" && make
RUN cp flatbuffers/flatc /usr/local/bin
```

```make
build-docker:
	docker build -t flatc -f docker/Dockerfile .

build-schema: build-docker
	docker run -it -v $(shell pwd):/schema flatc /bin/bash -c "cd /schema && \
	flatc -n --gen-onefile schema.fbs && \
	flatc -r --gen-onefile schema.fbs"
```

# Flatbuffers in Unity

TODO

# Alternatives

* protobuf
* cap'n'proto

comparison: https://capnproto.org/news/2014-06-17-capnproto-flatbuffers-sbe.html

# Further Resources

* [Improving Facebook’s performance on Android with FlatBuffers](https://engineering.fb.com/2015/07/31/android/improving-facebook-s-performance-on-android-with-flatbuffers/) (Article)
* [Bringing FlatBuffers Zero-Copy Serialization to Rust by Robert Winslow](https://www.youtube.com/watch?v=YsiQDX20lXI) (Video)