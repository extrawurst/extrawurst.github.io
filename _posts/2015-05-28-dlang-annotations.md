---
title:  "#dlang, annotations and menus"
date:   2015-05-28 18:00:00
categories: [dlang, metaprogramming, imgui]
---

What do menus and annotations have to do with the [D programming language](http://dlang.org/) ?
These three aspects form a very nice little system in my current spare time project of a game engine called [unecht](https://github.com/Extrawurst/unecht).

The whole engine borrows a lot of ideas and concepts from the [Unity3D game engine](http://unity3d.com/) and one of these concepts is the way the editor is extendable through user scripts in a conveniant way.

In any user defined class Unity3D allows you to define a static function that automatically appears in the editor menu when annotated like this:

{% highlight csharp %}

[MenuItem ("MyMenu/Do Something")]
static void DoSomething () {
    Debug.Log ("Doing Something...");
}

{% endhighlight %}

Clicking this menu entry will call the DoSomething function. I like this design a lot, it is simple and easy to grasp.

So unecht mimicks this behaviour fairly similar with code like this:

{% highlight d %}

@MenuItem("MyMenu/Do Something")
static void DoSomething () {
    ...
}

{% endhighlight %}

The only difference is that this code is compiled and that the whole magic for this to work happens at compile time.
In this implementation though I improved the design for validation functions (These functions get called to determine if a certain menu item should be enabled right now).
Unity3D makes you annotate these functions in a very specific way so that they match the name of the menuItem they correspond to. 
Errors like typos or a wrong signature are first caught at runtime.

In unecht it is as simple as adding a second parameter to the annotation which has to be another static method that returns a boolean:

{% highlight d %}

static bool ValidateDoSomething () {return true;}

@MenuItem("MyMenu/Do Something", &ValidateDoSomething)
static void DoSomething () {
    ...
}

{% endhighlight %}

If the validation function has a wrong signature or there is some typo these errors are all get caught already at compile time.

The end result is demonstrated here:
![menu demo]({{ site.url }}/assets/unecht-menu.gif)

Elegant implementations like these allowing such simple designs in the user code are what I like about D.
Although I am still missing a good getUDA/hasUDA method in the standard library to query for type annotations in D! It is a shame that these are being implemented by everyone in their libraries again and again.

For reference: the GUI itself uses [cimgui](https://github.com/Extrawurst/cimgui) (see my post about cimgui [here]({% post_url 2015-04-26-cimgui %})) wrapped for D based upon the fantastic [imgui](https://github.com/ocornut/imgui) c++ library by omar cornut.
I am using the about to be released 1.39 version here that supports the new menu API.