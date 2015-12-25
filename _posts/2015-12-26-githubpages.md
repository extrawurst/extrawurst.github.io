---
title:  "Extending github pages blog"
date:   2015-12-26 15:00:00
categories: [general]
---

### Categories Page

<img style="float: left; width: 250px;" src="{{ site.url }}/assets/blog-categories.png"/>

First I wanted to make use of the categories labels I kept adding to my posts. Since I am using github pages to host this site dynamic jekyll plugins are no option for me.

Fortunately I found the great [blog post](http://christianspecht.de/2014/10/25/separate-pages-per-tag-category-with-jekyll-without-plugins/) "Separate pages per tag/category with Jekyll (without plugins)" by Christian Specht.

Christian Specht writes about how to create a page per tag/category using jekyll without plugins. This was the foundation for my approach.

### Apps Page

<img style="float: left; width: 250px;" src="{{ site.url }}/assets/blog-apps.png"/>

I added an "Apps" page to this site: [Apps]({{ site.url }}/apps.html)

In order to do that the jekyll way you simply add a "layout" to the _layouts folder that uses markdown and html to define how to visualize the content (see [appspage.html](https://github.com/Extrawurst/extrawurst.github.io/blob/master/_layouts/appspage.html)). This is like a template. The actual content lies in raw form (yml) in the [apps.html](https://github.com/Extrawurst/extrawurst.github.io/blob/master/apps.html) file in the root.
