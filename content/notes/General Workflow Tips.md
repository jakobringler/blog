---
title: "General Workflow Tips"
tags:
- houdini
---

### Temporary Attributes
When creating HDAs or working on bigger setups it might make sense to  prefix any temporary attributes with a special character such as `__` to make removing it later on easier e.g. by using `__*` in an attribute delete SOP. 

By doing so you also ensure that you don't run into issues with incoming attributes that have the same name.

### Cache Node
The [Cache Node](https://www.sidefx.com/docs/houdini/nodes/sop/cache.html) can be used to optimize playback and store data until anything upstream changes. Pretty simple and obvious, but somehow I learned pretty late about this small but awesome node and I still see it rarely in tutorials or other peoples shared setups.

> [!Quote] Quote from SideFX Docs
> 
> lets you scrub otherwise sluggish animations in real time, play pop networks backwards, etc. because the animation is precomputed and stored in memory






