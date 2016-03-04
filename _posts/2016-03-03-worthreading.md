---
title:  "Worth Reading (Februar 2016)"
date:   2016-03-03 15:00:00
categories: [reading, unity, dlang]
---

The following stuff is what caught my eye in the last couple of weeks. Consider these kind of posts as an entry in my personal knowledge base ;)

### Unity

* "Integrating Visual Studio with Unity3d on Mac using UnityVS Tools" by Yun Zhi Lin ([link](https://www.yunspace.com/post/integrating-visual-studio-with-unity3d-on-mac-using-unityvs-tools/))
* "why foreach creates garbage in unity" by Giovanni Campo ([link](http://codingadventures.me/2016/02/15/unity-mono-runtime-the-truth-about-disposable-value-types/))
* "monodis" Dis/Assembling CIL Code (kind of like ILSpy for mono) ([link](http://www.mono-project.com/docs/tools+libraries/tools/monodis/))
* "Unity Debug with Rider-EAP On OSX" by Leroy Shirto ([link](http://www.leroyshirto.co.uk/2016/03/unity-debug-with-rider-eap-on-osx/))

### Article: "Compile-time Argument Lists" on dlang.org

This is an [awesome article](http://dlang.org/ctarguments.html) differntiating compiletime tuples from generic compile time argument lists that allow for crazy stuff like this in D:

{% highlight d %}
import std.meta;
alias Params = AliasSeq!(int, double, string);
void foo(Params); // void foo(int, double, string);
{% endhighlight %}

### Book: "D Web Development"

As I already twittered I finally received a copy of the book about D web development by Kai Nacke that I reviewed:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Great! My copy of &quot;D Web Development&quot; arrived - look who was a reviewer :P <a href="https://twitter.com/hashtag/dlang?src=hash">#dlang</a> <a href="https://t.co/S828YvJCs1">https://t.co/S828YvJCs1</a> <a href="https://t.co/Wx8cKNa4HT">pic.twitter.com/Wx8cKNa4HT</a></p>&mdash; Stephan Dilly (@Extrawurst) <a href="https://twitter.com/Extrawurst/status/702234903384031232">February 23, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

### Post: "Declarative OpenGL (sort of) in D" by profan

Great article about automatic wrapping of C based APIs using D's strong meta programming features:

[blog post](https://pagefault.se/dlang/2016/02/26/declarative-opengl-dlang/)

###Post: "Practical tips to make the most out of your first GDC" by Eline Muijres

Last but not least, a guide how to survive GDC San Francisco on Gamasutra:

[link to post](http://www.gamasutra.com/blogs/ElineMuijres/20160225/266607/Practical_tips_to_make_the_most_out_of_your_first_GDC.php)

Hope to see you there!