---
title: Deformation Wrangle vs Attribute Wrangle
draft: false
tags:
  - houdini
  - vex
---
## Transforming Geometry

Matrix transformations can be applied to "every" attribute in one go using the deformation wrangle.

![[notes/images/Pasted image 20230914131210.png]]

- left: base
- middle: attribute wrangle transform
- right: deformation wrangle transform

### Attribute Wrangle
- can apply the transformation only to one attribute at a time. In this example only `@P`

```C
matrix m = ident();
rotate(m, chf("angle"), 4);

v@P *= m;
```
### Deformation Wrangle
- takes care of vectors and quaternions (such as `N` or `orient`) and transforms/deforms/rotates them properly
- can only update point or vertex attributes (no primitives)

```C
pos = pos;
xform = xform;

matrix m = ident();
rotate(m, chf("angle"), 4);

pos *= m;
```

Check out Roc Andic's video I linked below to find out more!

---

sources / further reading:
- [Houdini VEX: What's so special about the deformation wrangle? - Roc Andic](https://www.youtube.com/watch?v=DRTk5nBr-aA)

