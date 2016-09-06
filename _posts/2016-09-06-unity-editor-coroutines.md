---
title:  "unity coroutines in the editor mode"
date:   2016-09-06 02:00:00
categories: [unity, general]
---

Did you ever try to do non-blocking WWW-requests in Unity without being in playmode ? Then you know about the problems here:

* WWW uses coroutines to be used non-blocking
* Coroutines do not work outside the playmode
* Triggering a coroutine in the editor using Update only works with an Editor-Window

The solution is twofold:

* Use [EditorApplication.update](https://docs.unity3d.com/ScriptReference/EditorApplication-update.html) to 'tick' in non-playmode
* Trigger coroutine manually

Pseudocode says more than a thousand words:

{% highlight csharp %}

IEnumerator coroutine;

void Init()
{
  EditorApplication.update += EditorUpdate;

  // StartRequest is build like a normal 
  // coroutine and yield returns on WWW
  coroutine = StartRequest();
}

void EditorUpdate()
{
  coroutine.MoveNext();
}

{% endhighlight %}

This does the trick and is not widely known unfortunately. Using EditorApplication.update we can register arbitrary callbacks that get called periodically in Editor-Mode.

Additionally understanding unity coroutines is necessary to understand what we are doing here:
A coroutine is nothing but a resumable method that is identified by the return type ```IEnumerator``` and the usage of ```yield return xyz;``` in its body.
Under the hood it works by 'someone' calling [MoveNext](https://msdn.microsoft.com/en-us/library/system.collections.ienumerator.movenext(v=vs.110).aspx) of its IEnumerable interface. Usually this 'someone' is Unity internally. But since that does not happen outside the playmode we can force it to still do the same by calling it manually.

Final note: [EditorApplication.timeSinceStartup](https://docs.unity3d.com/ScriptReference/EditorApplication-timeSinceStartup.html) is great for actual timing in the registered update-callback.

