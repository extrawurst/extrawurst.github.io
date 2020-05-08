---
title:  "terminal ui for git written in rust"
date:   2020-05-08 17:00:00
categories: [general]
---

Today it is time to announce [gitui](https://github.com/extrawurst/gitui):

![s0]({{ site.url }}/assets/gitui/s00-diff.png)

Over the last couple of weeks my side project was this little tool and some may know it from the occasional tweet about it already:

<blockquote class="twitter-tweet tw-align-center"><p lang="en" dir="ltr">gitui - my little <a href="https://twitter.com/hashtag/git?src=hash&amp;ref_src=twsrc%5Etfw">#git</a> terminal tool written in <a href="https://twitter.com/hashtag/rustlang?src=hash&amp;ref_src=twsrc%5Etfw">#rustlang</a> now supports staging/unstaging parts of a diff (hunks) and much more <a href="https://t.co/tuNcfsesfq">pic.twitter.com/tuNcfsesfq</a></p>&mdash; Stephan Dilly (@Extrawurst) <a href="https://twitter.com/Extrawurst/status/1252654046269366272?ref_src=twsrc%5Etfw">April 21, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

# What?

`gitui` is becoming my GUI alternative for using git. So far it only supports a few features but my goal is to extend it as needed. My focus is not to build a git cli substitute though, gitui is supposed to help out on tasks that are cumbersome to do on the cli for pure mortals like me. The main features so far:

* staging parts of files ((c)hunks)
* browsing the commit history
* inspecting large diffs/changesets
* reverting changes
* staging/unstaging file trees
* written in **Rust** for safety and speed
* it's free and on [github](https://github.com/extrawurst/gitui)

# Why?

**Q**: Why not use the raw `git` cli?  
**A**: Complex tasks are hard to do. I googled for how to rebase stuff interactively or stage hunks often enough to realise that some people just need something more visual.

**Q**: Why don't you use the GUI application `{insert name}`?  
**A**: I tried a lot of them, they do much *more than I need*, tend to be super *slow* and grinding to a halt on bigger repos and one after the other starts to cost money.

**Q**: Why not build yet another GUI application?  
**A**: GUI applications are complex, most of them are not free because of this. If you do most of your work in the terminal, its natural not to have to leave it for your git interaction. Plus it gives me this nice nostalgic feeling ;).

**Q**: Isn't there already such a tool?
**A**: The idea of a terminal UI for git isn't new, there is great examples out there: [tig](https://github.com/jonas/tig) and [lazygit](https://github.com/jesseduffield/lazygit)
They are both awesome and inspired me but either I found them too slow/resource-hungry in some areas or too complicated to use and navigate in others. (maybe a little [NIH-syndrome](https://en.wikipedia.org/wiki/Not_invented_here) as well).

**Q**: Why in *Rust*?  
**A**: I started using Rust two years ago, first out of fun, then professionally and so far there was no project to build something out in the open. `gitui` gave me the opportunity to opensource something built in Rust and I think it's a great fit for the kind of tool it is.

# Benefits of using Rust

**Disclaimer**: First of all, of course `gitui` could have been written in any language. I don't want to start language wars here.

My personal background is that I started out as a C++ programmer, system programming languages always appealed to me. Still I appreciated the comfort of managed systems like Java, C#, Node/Javascript and lately also Go. Over the years I kept watching out for something though that combined the strength of those two worlds. I think I found that in Rust:

1. **safety**
	* Rust has a unique memory model that offers me bare metal control without sacrificing safety
2. **speed**
	* It is a system level language, you can be as fast as C, people wrote os kernels in it, no GC, no pauses, the sky is the limit 🚀
3. **ergonomic**
	* closures, generics, functional, abstraction, modularization - everything but inheritance
4. **interop**
	* seamless c interop allows to tap into a huge ecosystem of existing libraries and frameworks
5. **ecosystem**
	* cargo+rust == npm+nodejs == gradle+java
	* I really don't know how I was surviving without such a rich and tightly integrated ecosystem of libraries in C/C++ before

Of course `gitui` is not re-inventing the wheel at all. It is standing on the shoulders of giants using the following major dependencies:

| what | why |
| -- | -- |
| [crossterm](https://github.com/crossterm-rs/crossterm) + [tui-rs](https://github.com/fdehau/tui-rs) | crossterm is a cross platform terminal library and tui-rs builds an immediate mode UI layer on top of that |
| [git2-rs](https://github.com/rust-lang/git2-rs) | git2-rs is a rust wrapper on top of [libgit2]() allowing to work with git natively (not having to call the commandline) |
| [rayon](https://github.com/rayon-rs/rayon) | on the topic of concurrency: `gitui` uses rayon for its threadpool |
| [crossbeam](https://github.com/crossbeam-rs/crossbeam) | more concurrency: crossbeam allows waiting (selecting) multiple channels in parallel |

There is much more to say about how `gitui` utilizes Rust's strengths but this post is getting out of hand already.
**More details will follow in the next post**

# Next steps

* join in on the fun and start contributing, its [opensource](https://github.com/extrawurst/gitui)
* check the [issues](https://github.com/extrawurst/gitui/issues) and file whatever you would like to see implemented
* for me up next is: **stashing** and **rebase**
* if you like what you see consider supporting my work on itch.io: [https://extrawurst.itch.io/gitui](https://extrawurst.itch.io/gitui)

![s1]({{ site.url }}/assets/gitui/s01-log.png)
