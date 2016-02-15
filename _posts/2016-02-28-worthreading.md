---
title:  "Worth Reading (Februar 2016)"
date:   2016-02-28 15:00:00
categories: [reading, unity, dlang]
---

The following stuff is what caught my eye in the last couple of weeks. Consider these kind of posts as an entry in my personal knowledge base ;)

###Post: "Integrating Visual Studio with Unity3d on Mac using UnityVS Tools" by Yun Zhi Lin

[UnityVS OSX](https://www.yunspace.com/post/integrating-visual-studio-with-unity3d-on-mac-using-unityvs-tools/)

###Article: "Compile-time Argument Lists" on dlang.org

Awesome article differntiating compiletime tuples from generic compile time argument lists that allow for crazy stuff like this:

{% highlight d %}
import std.meta;
alias Params = AliasSeq!(int, double, string);
void foo(Params); // void foo(int, double, string);
{% endhighlight %}

[Article](http://dlang.org/ctarguments.html)

[why foreach creates garbage in unity](http://codingadventures.me/2016/02/15/unity-mono-runtime-the-truth-about-disposable-value-types/)
[monodis IL disassembler for linux and osx](http://www.mono-project.com/docs/tools+libraries/tools/monodis/)