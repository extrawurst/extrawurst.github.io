---
title:  "#dlang Fibers and a lightweight eventloop"
date:   2015-05-30 21:00:00
categories: [dlang, gamedev, events, fibers]
---

###D and Fibers

[Fibers](http://dlang.org/phobos/core_thread.html#.Fiber) are D's concept for resumable functions and the barebone behind the popular event driven framework [vibe.d](http://vibed.org/).
In the [unecht](https://github.com/Extrawurst/unecht) project I recently added a generic system to use resumable functions much like [Unity3D](http://unity3d.com/) supports it using their [coroutine system](http://docs.unity3d.com/Manual/Coroutines.html).
In Unity3D a simple usage example is this:

{% highlight csharp %}
IEnumerator DeferedPrint() {
    Debug.Log("hello");
    yield return new WaitForSeconds(1f);
    Debug.Log("... 1s later");
}
// start the coroutine:
StartCoroutine(DeferedPrint);
{% endhighlight %}

The same behaviour in D and unecht looks like this:

{% highlight d %}
void DeferedPrint() {
    writefln("hello");
    UEFibers.yield(waitFiber!"2.seconds");
    writefln("... 1s later");
}
// start the fiber:
UEFibers.startFiber(DeferedPrint);
{% endhighlight %}

###Lightweight Eventloop

The eventloop necessary to allow this is simply resuming all active fibers each frame. 
The main addition to the Fibers in the standard library is that each Fiber can have a child Fiber that has to finish before the parent Fiber can resume. 
This way the Timer is realized.

{% highlight d %}
@MenuItem("test/deleteAll")
static void removeOnePerX()
{
    UEFibers.startFiber({
            while(ballRoot.sceneNode.children.length > 0)
            {
                UEFibers.yield(waitFiber!"200.msecs");
                removeBall();
            }
        });
}
{% endhighlight %}

This way the execution of a single function can be defered over a specific time span. The result of that example is demonstrated here:
![fibers demo]({{ site.url }}/assets/unecht-fibers.gif){: .center-image }

The whole code of this is available on github [here](https://github.com/Extrawurst/unecht/blob/master/source/unecht/core/fibers.d)