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

### Attribute Adjust Nodes

- Introduced in 18.5
- Very handy for all sorts of attribute manipulation, remapping or distorting
- Can do unit conversion from e.g. seconds to frames
- Check out [[notes/Attribute Adjust Syntax Tricks |Attribute Adjust Syntax Tricks]] for more

### Simulation Scale

What scale should you sim in?
Vex is only 32bit by default so you should scale your simulation to a size where you avoid huge and tiny values. Otherwise you will run into stability issues (e.g. particles escaping colliders etc.).
In FLIP for example, a good rule of thumb is to have your particle separation between 0.5 - 10 cm.

Values should stay between 16.000 and 1e-4 (0,0001) to avoid running into precission errors

