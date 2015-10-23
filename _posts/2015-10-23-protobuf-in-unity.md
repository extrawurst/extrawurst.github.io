---
title:  "Worth Reading (September 2015)"
date:   2015-09-30 20:00:00
categories: [reading]
---

I recently went through the hustle to setup protobuf support in a unity project (for an iOS game). This is a short guide on how to do it.

###Steps (overview)

* download [protobuf-net r668.zip](https://code.google.com/p/protobuf-net/)
* use protobuf-net's protogen to generate your C# code
* create separte DLL containing the generated C# code
* use protobuf-net's precompile to generate non-JIT compatible serialization code for the generated DLL (see the author's [blog post about it](http://blog.marcgravell.com/2012/07/introducing-protobuf-net-precompiler.html))
* write a test in unity

###Steps (in detail)

## download protobuf-net

Note: Since r668 is dated to 2013 it won't support protobuf 2.6 which means essentially 'oneOf'.

## Test in Unity

{% %}

{% %}