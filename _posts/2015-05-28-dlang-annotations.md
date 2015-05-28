---
title:  "#dlang, annotations and menus"
date:   2015-05-28 21:00:00
categories: [dlang, metaprogramming, imgui]
---

What do menus and annotations have to do with the D programming language ?
These three aspects form a very nice little system in my current spare time project of a game engine called [unecht](https://github.com/Extrawurst/unecht).

The whole engine borrows a lot of ideas and concepts from the [unity3D game engine](http://unity3d.com/) and one of these concepts is the way the editor is extendable through user scripts in a conveniant way.

In any user defined class unity3D allows you to define a static function that automatically appears in the editor menu like like this:

{% highlight csharp %}

[MenuItem ("MyMenu/Do Something")]
static void DoSomething () {
    Debug.Log ("Doing Something...");
}

{% endhighlight %}

Clicking this menu entry will call the SoSomething function. I like this design a lot. It is simple and easy to grasp.

So unecht mimicks this behaviour fairly similar with code like this:

{% highlight d %}

@MenuItem("MyMenu/Do Something")
static void DoSomething () {
    ...
}

{% endhighlight %}

The only difference is that this code is compiled and that the whole magic for this to work happens at compile time.
In my implementation though I improved the design for validation functions: In Unity3D those functions are annotated slightly differently and have to be named exactly like the actual method and return a boolean to enable/disable the menu entry. Errors like typos are first caught at runtime with that design.

In unecht it is as simple as adding a second parameter to the annotation which has to be another static method that returns a boolean:

{% highlight d %}

static bool ValidateDoSomething () {return true;}

@MenuItem("MyMenu/Do Something", &ValidateDoSomething)
static void DoSomething () {
    ...
}

{% endhighlight %}

The end result is visible here:
[]()

Elegant implementations like these allowing such simple designs in the user code are what I like about D.