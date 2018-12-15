---
title:  "make alexa toast in go"
date:   2018-12-15 9:00:00
categories: [general]
---

I always looked for a chance to dig into the `go` programming language. Recently I found the perfect opportunity: writing an **Alexa Skill in go**!

![skill]({{ site.url }}/assets/alexa-prost.jpg)


## Why go?

Why did I feel the urge to look into go? Because I like to keep up to date with the latest programming languages and developments.

One other reason why I chose go was: **AWS lambda support** - out of a bunch of scripting/VM based languages go was the only system programming language that is natively supported. Alexa Skills are best developed on lambda and since I was fed up using JS it was go this time.

go is a tool in a toolbox, this particular one is trying to be a system programming language (no VM) and to be more modern than crusty old C/C++. 
Like any other tool, it is solving a particular purpose. go's primary purpose is to be *modern* (having a GC and all) while still cater the needs of system developers (*runtime speed* and statically typed'ness) and still be attractive to script programmers (fast iteration - aka. *quick compilation*). On top of that, go wants to simplify *concurrency* (don't dare to call it [parallelism](https://www.youtube.com/watch?v=cN_DpYBzKso)) with built in support for coroutines (ahem, I mean goroutines) and message passing using channels.

## First impressions

*TL;DR*

* ❤️ really easy setup
* ❤️ all-in-one installation
* ❤️ easy cross compilation
* 👍 imports == dependencies
* 👍 tight git integration
* 👍 multiple return values
* 👍 somewhat familiar syntax
* 👎 no versioned dependencies *
* 💔 no deterministic builds *
* 💔 workspace concept feels weird *
* 💔 `err` everywhere
* 😱 no generics

\* Those are worked on and with version 1.11 and it's modules concept much of this might disappear ([see my post on this](https://blog.extrawurst.org/general/2018/12/05/go-and-gitlab-ci.html)).

*Disclaimer:* I want to emphasize that I am not a go expert by any means, so take these with a grain of salt.

### Generics 

For me, the lack of generics was the biggest downer because it introduces other disadvantages. For example, I was looking for a HashSet implementation in go - guess what: does not exist - because of missing generics. If you need such a container, write it exactly for the one type that you need it for.

This means that for most things you either take built in stuff or you have to write it specially tailored for yourself - I ended up abusing `map` as a HashSet 🙈.

### Error handling

Error Handling is my second biggest concern. go wanted to rise from the past of crusty old C and still they implemented error handling in a very C-ish way. Everything returns an error-code or object. Thanks to multiple return values, this is not as bad as in C, but still bloats my code a lot. 

```go
sess, err := awssession.NewSession(...)

if err != nil {
    log.Fatalf("session error: %s\n", err)
    return nil
}
```

I ended up checking for `err != nil` everywhere and felt like it is tempting to plaster my code with `panic()` everywhere, as an easy out. I am not sure this concept proves good, since the dirty/easy way out is so damn tempting.

## Alexa skills in go

My use case for go was writing an Alexa Skill, remember?

Unfortunately AWS does not offer an off-the-shelf SDK for alexa skills in go like it does for javascript but thanks to open source this does not matter much: [alexa-skills-kit-golang](https://github.com/ericdaugherty/alexa-skills-kit-golang)

Therefore writing a skill in go is similarly easy: 
```
func (h *HelloWorld) OnLaunch(ctx context.Context, request *alexa.Request, session *alexa.Session, ctxPtr *alexa.Context, response *alexa.Response) error {
	speechText := "Welcome to the Alexa Skills Kit, you can say hello"

	response.SetOutputText(speechText)

	return nil
}
```

Fortunately accessing other AWS services like DynamoDB is even easier, there is an official SDK: [aws-sdk-go](https://github.com/aws/aws-sdk-go)

Why do I need DynamoDB access? Because I need to persist the last jokes my skill response uses to not repeat her over and over again but prefer not used sentences.

## Conclusion

Thanks to this all-in-one easy setup, I was productive very quickly and thanks to the 'golang' nickname, it was easy to find on the internet (good luck searching for `go`) - after all, go is not that new.

I am not sure how much further I want to dig into go. Although I want to look more into the concurrency model before departing. I feel like go is a good middle ground for small to mid size projects and the blazing fast compilation is definitely something I would miss. 

In certain areas I wish go would get away even further from its C-ancestors, but maybe I will think differently once I checked out `Rust` 😊.

But for now, go remains my `goto`-language 😉 when it comes to AWS lambda functions. IMHO it's the best in the pool of natively supported languages. It is statically typed, compiled, safe and blazing fast.