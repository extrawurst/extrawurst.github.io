---
title:  "Worth Reading (March 2015)"
date:   2015-03-14 18:30:00
categories: [reading, programming, cpp]
---

The following stuff is what caught my eye in the last couple of weeks. Consider this post an entry in my personal knowledge base ;)

### Talk about Efficiency at CppCon2014

Chandler Carruth gave a talk named "Efficiency with Algorithms, Performance with Data Structures" at CppCon2014 that I just recently saw and advice everybody to watch.

<iframe src="//channel9.msdn.com/Events/CPP/C-PP-Con-2014/Efficiency-with-Algorithms-Performance-with-Data-Structures/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

He talks about effective algorithms and performance of data structures on modern CPUs in our post-moore's-law-world. He considers Linked-Lists and HashMaps evil and advises us to use std::vector whereever possible. But don't think there is nothing to learn there if you are a java guy!

### DOD and Entity Systems

These topics are somewhat related and caught my interest especially in the last couple of weeks. 
Data Oriented Design is related to the talk above because it tries to optimize data structures for fast cache aware processing. 
Entity Systems are the foundation for game engines like unity3D that model the game logic in a very modular and decoupled way.
Artemis is a very nice example of an Entity System Framework written in Java. 
I whish I find the time to make use of these very interesting concepts in my next game project.

* [Data Oriented Design Presentation](https://www.youtube.com/watch?v=16ZF9XqkfRY)
* [Blog posts about Entity Systems](http://t-machine.org/index.php/2007/09/03/entity-systems-are-the-future-of-mmog-development-part-1/)
* [artemis website](http://gamadu.com/artemis/index.html)
* [GDC Talk: Data Driven Gameobject System](http://scottbilas.com/files/2002/gdc_san_jose/game_objects_slides.pdf)
