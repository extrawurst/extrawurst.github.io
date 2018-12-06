---
title:  "alexa skill in go"
date:   2018-12-06 9:00:00
categories: [general]
published: false
---

I always looked for a chance to dig into the `go` programming language. Recently I found the perfect opportunity: writing an **Alexa Skill in go**!

![skill]({{ site.url }}/assets/alexa-prost.jpg)


## Why go?

Why did I feel the urge to look into go? Because I like to keep up to date with the latest programming lanuages and developments.

go is a tool in a toolbox, this particular one is trying to be a system programming language (no VM) and be more modern than crusty old C/C++. 
Like any other tool it is solving a particular purpose. go's primary purpose is to be *modern* (having a GC and all) while still cater the needs of system developers (*runtime speed* and statically typed'ness) and still be attractive to script programmers (fast iteration - aka. *quick compilation*). On top of that go wants to simplify *concurrency* (dont date to call it [parallelism](https://www.youtube.com/watch?v=cN_DpYBzKso)) with build in support for coroutines (ahem I mean goroutines) and message passing using channels.

## First impressions

*TL;DR*

* ❤️ really easy setup
* ❤️ all in one installation
* ❤️ easy cross compilation
* 👍 imports == dependencies
* 👍 tight git integration
* 👍 multiple return values
* 👍 somewhat familliar syntax
* 👎 no versioned dependencies *
* 💔 no deterministic builds *
* 💔 workspace concept feels weird *
* 💔 `err` everywhere
* 😱 no generics

I want to emphasize that I am not go expert my anymeans so take this with a grain of salt.

\* those are worked on and with version 1.11 and the modules concept much of this might disappear: see also my other post

### Generics 

For me generics were the biggest downer because it also brings a lot of other disadventages. I was looking for a HashSet implementation in go, guess what: nope - because no generics. If you want one write one exactly for the one type that you need it for.

This means that for most things you either take build-in stuff or you have to write it specially tailored for yourself - I ended up abusing `map` as a set 🙈.

### Error handling

This is my second biggest concern. go wanted to rise from the past of crusty old C and still they implemented error handling in a very C-ish way. Everything returns an error-code or object. Thanks to multiple return values this is not as bad as in C but still bloats my code a lot. 

```go
sess, err := awssession.NewSession(...)

if err != nil {
    log.Fatalf("session error: %s\n", err)
    return nil
}
```

I ended up checking for `err != nil` everywhere and feel like it is tempting to plaster my code with `panic()` everywhere as an easy out. I am not sure this concept proves good if the dirty/easy way out is so damn tempting.

## Conclusion

Thanks to this all-in-one easy setup I was productie very quickly and thanks to the 'golang' nickname help was easy to find on the internet - after all go is not that new.

I am not sure how much further I want to dig into go. Although I want to look more into the concurrency model before departing. I feel like go is a good middleground for small to mid size projects and the balzing fast compilation is definetely something I would miss. 

In certain areas I whish go would get away even further from its ancestors but maybe I will think differently once I checked our rust 😊.

But for now go remains my `goto`-language 😉 when it comes to AWS lambda functions, since its for me the best in the pool of natively supported ones.