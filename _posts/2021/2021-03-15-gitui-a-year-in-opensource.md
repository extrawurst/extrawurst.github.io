---
title:  "GitUI - 1 year in open source"
date:   2021-03-15 9:00:00
categories: [general,programming,rust]
---

# Time races

Exactly one year ago I commited the first version of [GitUI](https://github.com/extrawurst/gitui) to Github. It's crazy how quickly the time passed by and now we celebrate the first year anniversary🥳.

![demo]({{ site.url }}/assets/gitui-anniversary/demo.gif)

# Time to reflect

This project attracted way more attention than i anticipated and so I eventually found myself reviewing contributions by others❤️!

What a great feeling. This is why you publish something open source, right?

Well it is a double-edged sword: 
With every contributor you get a handful of issues and feature requests, some make sense, some are interesting challenges, some are ridiculous, some are just not important to you, still it is a good feeling to see your creation being noticed. 

You learn to say **no**. Well technically it is more like a *nice idea, not high up on my list though, contributions welcome* instead of *no*.

# Learnings

My 10 most important take-aways:

0. Be nice and welcoming to everyone who contributes, even the smallest issue.
1. Provide a roadmap, so contributors can see the big picture.
2. Be prepared that people will try to fork away and think they can do better.
3. __Some__ users will think you just wait to fix *their* issues for *them*.
4. There will be low times, stuff will pile up - learn to accept that don't feel pressured.
5. At times its tempting to *just* accept a PR & finally move on - do not give in - keep your quality standard.
6. Number 5. will annoy contributors that seek a quick'n'dirty *contributor* badge, but it is a good natural filter.
7. It is a collosal enjoyment to see first time contributors grow to be a reliable source of support.
8. No hard feelings when people leave again, do not finish their work, do not show ownership, lose interest.
9. Ask for help, it is surprising how humble-ness mobilizes people.

# In numbers

**30 amazing contributors ❤️**

![contributors]({{ site.url }}/assets/gitui-anniversary/contributors.png)

**github stars over time ⭐️**

![stars]({{ site.url }}/assets/gitui-anniversary/stars.png)

**130 forks 🍴**

![forks]({{ site.url }}/assets/gitui-anniversary/forks.png)

**~18.000 lines of rust code 🦀**

![loc]({{ site.url }}/assets/gitui-anniversary/loc.png)

# The road to 1.0

My initial goal was to match features that i found annoying in the regular git-cli. Use-cases that I resort to solve in GUIs like Fork. We have long passed that point thanks to contributions. Still I now formulated my top priorities to get done before being able to call gitui version **1.0**:

* upstream branches 

    right now its not possible to check what branches are present in remotes and check them out locally

* merging with conflicts

    right now we only allow merging (after pull) when it is possible without creating conflicts

* log search (commit, author, sha)

    particularly tough for giant repos but good way to shine over alternatives: *Fork for example simply stops searching at a certain point*.

* file history log

    filter the commit log by one single file.

* more tag support

    list tags, jump to tags in the commit log, delete a tag

* file blame

    classic git feature: allows inspecting each line by when and who it was contributed.

* visualize branching structure in log tab

    probably one of the toughest features: visualize the tree structure of the repo in the commit log. this will also unlock features like: cherry picking from other branches

Feel free to join in on the fun! You are no rust developer? No worries, its a good way to start with our [good-first-issues](https://github.com/extrawurst/gitui/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22) and we help you navigate the pitfalls.

You like what GitUI became? Consider sponsoring to speed up development: [Github Sponsors](https://github.com/extrawurst/gitui) ❤️