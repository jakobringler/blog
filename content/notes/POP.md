---
title: POP
draft: false
tags:
  - houdini
  - fx
---
### Initializing Attributes inside DOPs

Sometimes you want to initialize / build some attribute based on the geometry of the frame before. To do so you can plug it in the second input of the pop solver and then use it in the third. 

![[notes/images/Pasted image 20231018160005.png]]

This is very useful because the only thing that happens in the pop solver before processing the second input (yellow) is the reaping of unnecessary particles to avoid overhead. After that the usual pop solver micro solvers are run and in the end all the user instructions for the next frame are applied (green).

![[notes/images/Pasted image 20231018160140.png]]

---

sources / further reading:
- 

