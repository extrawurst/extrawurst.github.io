---
title:  "Github Pages"
date:   2015-03-14 16:30:00
category: general
---

Recently I decided to start a new blog/website based on Github Pages.
For now this site runs in parallel with my old website [www.extrawurst.org](www.extrawurst.org) but eventually this pages is gonna be shutdown.
This post is about my first impressions using Github Pages!

TL;DR; These are my findings:
 
* Easy to setup
* Easy to edit
* Git Integration
* No hosting hustle

--- Setup ---

The setup was extremly easy following the steps on github itself: [https://pages.github.com](https://pages.github.com)

Github Pages supports jekyll to design and write your static pages: [http://jekyllrb.com/](http://jekyllrb.com/)

Then I browsed jekyll themes here: [http://jekyllthemes.org/](http://jekyllthemes.org/)

Finally I choose one and simply forked it on github - nifty!

To be able to test the site and write posts locally befor committing you can setup jekyll on your machine: 

{% highlight sh %}
~ $ gem install jekyll
~ $ jekyll new my-site
~ $ cd my-site
~/my-site $ jekyll serve
# => Now browse to http://localhost:4000
{% endhighlight %}

Ok then I thought "how to enable comments by the readers on a statically deployed site?" -  Here [disqus.com](https://disqus.com/) comes to the rescue and enables to outsource the whole commenting feature to a service - nifty again!

--- Editing ---

Posts are written in markdown just like wiki pages on Github. For those like me who write markdown just rarely enough not to remember the details here are the docs: [Markdown](http://daringfireball.net/projects/markdown/syntax)

--- Git Integration ---

Since Github Pages deploys the website right from your github repo publishing a new post is just a matter of commit/push - nifty!
Then again maybe this is a feature that only developers really can appreciate ;)

--- No hosting hustle anymore ---

That is the main reason for me to switch. I always have periods where I have time to post occasionally and keep my page up to date but then there comes the time when I don't and then my old wordpress page gathered a backlog of updates and eventually unmaintained plugins that are rendered incompatible... sigh. Then there is always the problem with keeping the vserver up to date and the security risks arising from self hosting. All this is gone with this approach and since I don't mind for my blog to be open source Github Pages is a perfect fit for me!

