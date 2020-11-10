---
title:  "Scalable Architecture for Unity Games"
date:   2020-11-10 13:00:00
categories: [gamedev,unity,programming]
draft: true
---

# Vision

Over the years and over the course of many projects a certain way to structure a game project in Unity emerged that proved particularly scalable (maintainable).

![overview]({{ site.url }}/assets/unity-arch/overview.jpeg)

For a long time I wanted to write this up to bring it into a sharable format.

This article is an update to a talk I gave at [GDC in 2017](https://www.gdconf.com) ("Data Binding Architectures for Rapid UI Creation in Unity").

**Disclaimer:** Of course these are only best practices that worked for me and reflect my experience and opinion, these are no silver bullets for everything and definitely not the right approach for every project or team. 

**Disclaimer2**: After writing this people made me aware that we are in good company with this approach since Kolibri Games applies something similar aswell: [Blog Post](https://www.kolibrigames.com/blog/making-games-the-lean-way-part-1/)

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

## Inversion of Control

The following diagram illustrates how tightly coupled components usually work:

![coupling]({{ site.url }}/assets/unity-arch/coupling.png)

`ClassA` directly depends on `ServiceA`/`ServiceB`. This makes it harder to independently test `ClassA` without having to worry about the implementation details of the services.

Dependency Injection (DI) is an approach to apply *inversion of control*. The following graphic illustrates the previous example using DI:

![di]({{ site.url }}/assets/unity-arch/di.png)

DI is used as a *Builder* to generate our `ClassA`, inspecting the necessary dependencies and resolving these automatically. `ClassA` does not care what concrete class is used that implements the required `interface` as long as there is one.

We settled on using [Zenject/Extenject](https://github.com/svermeulen/Extenject) to apply this pattern. It is reflection based. Using the reflection-baking feature we can get rid of the reflection related performance impact.

## Model-View-Controller

The heart of this architecture is breaking up the code into seperate layers. The Model-View-Controller (MVC) design applied to Unity looks as follows:
![mvc]({{ site.url }}/assets/unity-arch/mvc.png)

Unity Monobehaviours live in the **View**-Layer and are supposed to shield the rest of the architecture from the hard to unittest elements of Unity. This layer only has access to the **Control**-Layer. The **View** creates instances of prefabs, uses `[SerializeField]` to use Unity typical drag/drop components. No logic is encoded in here, pure visualization of data only.

The **Controller**-Layer contains the business logic and does the heavy lifting. This code is supposed to be testable, it does not depend on Unity **View** specifics. This layer still does not define the way data is stored in the **Model**-Layer, it is only controlling changes on it.

The **Model** contains the actual Data, this might be ephemeral, on disk or in some backend. Usually these Models are Plain-Old-Data types.

Since the **View** should not poll for changes on the data we use *Message Passing* to notify it. This way we can keep the Layers decoupled and still contain performance.

## Message Passing

The above design relies on appropriate notification messages so that the View-Layer can subscribe and react to changes/events:
![messagebus]({{ site.url }}/assets/unity-arch/messagebus.png)

We are using [Zenject Signals](https://github.com/modesttree/Zenject/blob/master/Documentation/Signals.md).

The following code example shows usage of it:

```cs
struct MessageType {}

bus.Subscribe<MessageType>(()=>Debug.Log("Msg received"));

bus.Fire<MessageType>();
```

It is important to note that Signals are supposed to be *lightweight* and do not contain *Data* - we use the rest of the MVC-Layers for this. Signals are a tool for pure notification, event propagation and decoupling.

## Unittesting

Based on all the efforts above we can now write unittests for almost all of our game (business) logic.

On the technical side of writing those tests we use the Unity standard [NUnit](https://nunit.org) framework and [NSubstitute](https://nsubstitute.github.io) as a mocking solution.

Let's have a look at one of our tests:

```cs
var level = Substitute.For<ILevel>();
var buildings = Substitute.For<IBuildings>();

// test subject: 
var build = new BuildController(null,buildings,level);

// smoke test
Assert.AreEqual(0, build.GetCurrentBuildCount());

// assert that `GetCurrent` was exactly called once
level.ReceivedWithAnyArgs(1).GetCurrent();
```

The above test is only making sure the controller behaves correctly when fed with default data. You can still learn how we are using *NSubstitute* to mock dependencies and later even assert that specific methods were called on these.

Let us look at a more intersting example of actually building something on slot `0`:

```cs
var level = Substitute.For<ILevel>();
var buildings = new MockBuildingModel();
var bus = _container.Resolve<SignalBus>();
var buildCommandSent = false;
bus.Subscribe<BuildingBuild>(() => buildCommandSent = true);

// test subject: 
var build = new BuildController(bus,buildings,level);

build.Build(0);

Assert.AreEqual(1, build.GetCurrentBuildCount());

// assert signals was fired
Assert.IsTrue(buildCommandSent);
```

Now you see how we can make sure the expected signal was even fired and that our `GetCurrentBuildCount` returns the correct new building count.

These sort of tests are inexpensive to run and keep the integrity of our game logic because we check that those are green even before building a new test version.

# Conclusion

This was just a 20.000ft view on this topic. 
Let's recap:

We want to be able to write testable code, therefore we decouple unity as much as possible from our business logic, we communicate to unity via messages, we have a clear interface from Unity how to access data. With this we have a small surface area of what is unity specific and cannot be tested (lets ignore playmode tests for now).

In future articles we will write a concrete example game to apply the above system and furthermore look how to combine this architecture with:

* mocking scenes for ui testing
* fake backends and third party SDKs
* promises for maintainable async code