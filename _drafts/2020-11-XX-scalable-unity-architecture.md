---
title:  "Scalable Architecture for Unity Games"
date:   2020-11-08 10:00:00
categories: [gamedev,unity,programming]
draft: true
---

# Vision

Over the years and over the course of many projects a certain way to structure a game project in Unity emerged that proved particularly scalable (maintainable).

For a long time I wanted to write this up to bring it into a sharable format.

This article is an update to a talk I gave at [GDC in 2017](https://www.gdconf.com) ("Data Binding Architectures for Rapid UI Creation in Unity").

**Disclaimer:** Of course these are only best practices that worked for me and reflect my experience and opinion, these are no silver bullets for everything and definitely not the right approach for every project or team.

# The Architecture

The main goals of this architecture are:

* maintainability
* extensibility
* testability

These three are not easy to achieve in an engine that is primarily inviting to rapid prototyping. The common conception among game developers is that these principles are more relevant to business solutions than in games and I strongly disagree. Games became more and more software as a service businesses. Allowing us to look at solutions in such areas we find that there is actually tools that we can apply to gaming aswell.

1. inversion of control
2. message passing
3. model / view / controller (MVC)
4. unittests

## Inversion of control

The following diagram illustrates how tightly coupled components usually work:

![coupling]({{ site.url }}/assets/unity-arch/coupling.png)

`ClassA` directly depends on `ServiceA`/`ServiceB`. This makes it harder to independently test `ClassA` without having to worry about the implementation details of the services.

Dependency Injection is an approach to do *inversion of control*. The following graphic illustrates the previous example using DI:

![di]({{ site.url }}/assets/unity-arch/di.png)

DI is used as a *Builder* to generate our `ClassA`, inspecting the necessary dependencies and resolving these automatically. `ClassA` does not care what concrete class is used that implements the required `interface` as long as there is one.

We settled on using [Zenject/Extenject](https://github.com/svermeulen/Extenject) to apply this pattern. It is reflection based but using the reflection-baking feature we can get rid of reflction related performance issues.

## Message Passing

![messagebus]({{ site.url }}/assets/unity-arch/messagebus.png)

* Zenject Signals

## MVC

![mvc]({{ site.url }}/assets/unity-arch/mvc.png)

* M + C + Unity
* TODO

## Unittests

* NUnit
* NSubstitude

# Next steps

This was just a 20.000ft view on this topic. 
Let's recap:

We want to be able to write testable code, therefore we decouple unity as much as possible from our business logic, we communicate to unity via messages, we have a clear interface from Unity how to access data. With this we have a small surface area of what is unity specific and cannot be tested (lets ignore playmode tests for now).

* mocking scenes
* fake backends
* promises