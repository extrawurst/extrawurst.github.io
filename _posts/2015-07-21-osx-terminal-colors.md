---
title:  "osx terminal colors"
date:   2015-07-21 15:00:00
categories: [general, osx]
---

This short post is just a wrap up of how to setup linux like terminal (bash) colors on osx.

This is how the terminal coloring is default on osx:
![terminal colors default]({{ site.url }}/assets/term-colors-befor.png)

Simply edit or create the bash_profile file like so:

{% highlight sh %}
vim ~/.bash_profile
{% endhighlight %}

and paste the following in there:

{% highlight sh %}
export PS1="\[\033[36m\]\u\[\033[m\]@\[\033[32m\]\h:\[\033[33;1m\]\w\[\033[m\]\$ "
export CLICOLOR=1
export LSCOLORS=ExFxBxDxCxegedabagacad
alias ls='ls -GFh'
{% endhighlight %}

This is from a blog post [here](http://osxdaily.com/2013/02/05/improve-terminal-appearance-mac-os-x/).

The end result will look like this:
![terminal colors after]({{ site.url }}/assets/term-colors-after.png)