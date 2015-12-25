---
title:  "Worth Reading (December 2015)"
date:   2015-12-25 15:00:00
categories: [reading, unity]
---

The following stuff is what caught my eye in the last couple of weeks. Consider these kind of posts as an entry in my personal knowledge base ;)

###Posts regarding Unity

["10000 UPDATE() CALLS"](http://blogs.unity3d.com/2015/12/23/1k-update-calls/)

Useful insights into Unity internals and how to profile/optimize certain areas of code.

["AN INTRODUCTION TO IL2CPP INTERNALS"](http://blogs.unity3d.com/2015/05/06/an-introduction-to-ilcpp-internals/)

Very interesting insights in il2cpp backend and how to avoid certain performance pitfalls.

###Website: "Vulkan Tutorial"

[Website](http://vulkan-tutorial.com/)

Vulkan is coming soon and this is a good starting point of how to use it and what it introduces. Especially interesting is SPIR-V the new IL for shader programming.

###Post: "Understanding C by learning assembly" by David Albert

[Blog Link](https://www.recurse.com/blog/7-understanding-c-by-learning-assembly)

Never used assembly ? This is a good starting point to understand system level programming by inspecting the generated assembly. I suggest every descent programmer should be familiar with assembly since even in times of highlevel VMs it all boils down to assembly in the end. This bring implications even on the highest level that you should be familiar with.

###rbenv

Making Jekyll work again on my mac was a strange hustle and it came down to the version of ruby preinstalled with osx. rbenv simplifies managing ruby versions and this instructions shows you how:

[usage instructions](https://github.com/rbenv/rbenv#homebrew-on-mac-os-x)

**note**: do not forget to reshash after installing the jekyll gem:
{% highlight bash %}
$ rbenv rehash
{% endhighlight %}