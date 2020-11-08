---
title:  "Scalable Architecture for Unity Games"
date:   2020-11-08 18:00:00
categories: [general,gamedev,unity,programming]
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
2. model / view / controller (MVC)
3. message passing
4. unittests

## Inversion of control

* Zenject
* reflection baking

## MVC

* M + C + Unity

## Message Passing

* Zenject Signals

## Unittests

* NUnity
* NSubstitude

# Next steps

This was just a 20.000ft view on this topic. 
Let's recap:

We want to be able to write testable code, therefore we decouple unity as much as possible from our business logic, we communicate to unity via messages, we have a clear interface from Unity how to access data. With this we have a small surface area of what is unity specific and cannot be tested (lets ignore playmode tests for now).