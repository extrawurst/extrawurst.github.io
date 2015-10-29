---
title:  "Worth Reading (October 2015)"
date:   2015-10-29 19:00:00
categories: [reading, dlang]
---

The following stuff is what caught my eye in the last couple of weeks. Consider these kind of posts as an entry in my personal knowledge base ;)

###Book: "Endurance: Shackleton's Incredible Voyage"

[Amazon.com link](http://www.amazon.com/Endurance-Shackletons-Incredible-Alfred-Lansing/dp/0465062881)

I found this book by Alfred Lansing absolutely stunning. Both a great adventure and a documentary about one of the most spectacular expeditions in modern history. It tells the story of an trans-antartic expedition turned into an breathtaking adventure of an inspiring leader trying to save the lives of his crew. This story is not only tense and freaking real it is an extraordinary example of inspiring leadership!

###Post: "SpringSource Certified Spring Professional" by Jakub Staš

[Blog Post](http://jakubstas.com/springsource-certified-spring-professional)

I was looking for information on a Spring Certification and stumbled upon this very interesting blog post from a guy who took this certification too.

###Post: "Using Google Protocol Buffers with Spring MVC-based REST Services"

[Spring.io Post](https://spring.io/blog/2015/03/22/using-google-protocol-buffers-with-spring-mvc-based-rest-services)

In this post it is described how to make Spring able to return webrequest results in protobuf format automatically.

Note: Maybe worth mentioning to be sure to add maven project "protobuf-java-format" into the dependencies to be actually able to convert proto results to json etc.

###Sound Library: "SoLoud" by Jari Komppa

Thanks to a twitter tweet (that I can't seem to find again) I stumbled upon this nice little sound library: 
[SoLoud](http://sol.gfxile.net/soloud/index.html)

It is crossplatform and feature most of the features that an indie dev needs and more. I was always looking for a really free alternative to [fmod](http://www.fmod.org/) and will definitely look into this as soon as I find the time. Especially since it features a [dlang](http://dlang.org/) binding right off the shelve: [SoLoud D-API](http://sol.gfxile.net/soloud/d_api.html).

###Post: "es_core: an experimental framework for low latency, high fps multiplayer games" by TTimo

[Blog Post](http://ttimo.typepad.com/blog/2013/05/es_core-an-experimental-framework-for-low-latency-high-fps-multiplayer-games.html)

TTimo talks about an interesting multiplayer game engine design specificly with low latency FPS multiplayer games in mind. He uses a highly concurrent design with no data sharing but message passing only while avoiding copys and "all memory heap operations". I like this design, I maybe adopt some of it in [unecht](https://github.com/Extrawurst/unecht) once I finally have some time to work on that again!
