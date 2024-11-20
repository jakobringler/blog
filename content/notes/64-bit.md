---
title: "64-bit"
draft: false
tags:
- houdini
---
## Make Houdini process data in 64-bit
This is a write up of Jeff Lait's [masterclass](https://vimeo.com/381044402) about 64-bit Processing in Houdini.

### The Problem
Usually Houdini works with 32 bit data. To show the limitations you can transform some geo a couple million units away from the origin and then back. Values will get quantized and data will be lost, since those numbers go past floating point limitations. To fix this you can use the `Attribute Cast` node which specifies which attribute values should be calculated using 64 bit.

### How to tell if something is in 64-bit "mode"
In the geometry spreadsheet as well as the mousewheel / inspection menu the attribute is displayed with a small 64 to indicate it's precision.
![[notes/images/64bit.png]]
Also there is a intrinsic detail attribute called precision that defines the preferred precision to be used downstream.

### VEX still 32-bit?
To make vex process in 64-bit you have to change the precision option under the bindings tab.

There is no 'double'-type in vex. It only depends on the precision chosen. If it's set to 64-bit, everything will be 64 bits long (int, float, vector etc.). This also ensures that the code won't need any changes when switching modes.

### Preferred Precision
Since many old nodes were built to work with 32-bits SideFX introduced a global precision override to avoid having to change all precision settings. You can set this global preferred precision by using an `Attribute Cast` node again.

The mode is indicated by a small `64` badge next to the node.

### Creating new attributes while in 64-bit precision
New attributes will be created in the correct precision depending on the currently preferred one. (Works with wrangles and attribcreate nodes)

### When to use 64-bit processing
1. Geometry moves far away from the origin
2. You have a very large amount of geometry (think fur or feather grooms) -> anything above 2 billion points
3. opencl types get set differently depending on precision mode used


### Vellum Simulations
To make Vellum compute in 64-bit modes you just have to feed 64-bit geometry into the solver / constraint nodes.
GPU preformance may suffer a lot doing this.

---

sources / further reading:
- [64-bit Processing in Houdini 18.0 - Jeff Lait (Houdini)](https://vimeo.com/381044402)

