---
title: "Quaternions & Euler, Radians & Degrees"
tags:
- houdini
- math
- vex
---

### Quaternions

expressed as 4 numbers `vector4 = [x, y, z, w]

It's usually used to define rotational transformation in 3D Space. To do so it needs 2 types of information: 
- rotational angle   $\theta$
- rotational axis   $A$ 

The 4 vector values of a quaternion are calculated in the following way:

$(sin(\frac{\theta}{2})*A.x, sin(\frac{\theta}{2})*A.y, sin(\frac{\theta}{2})*A.z, cos(\frac{\theta}{2}))$

In VEX we can use the quaternion function which accepts an angle in radians and an axis vector to propagate the vector4 accordingly.


### Rotating Vectors

```C#
//this goes in a point wrangle

float angle = chf("angle");
vector axis = normalize(chv("axis"));

vector4 rot = quaternion(radians(angle), axis);
@P = qrotate(rot, @P);
```

### Euler Rotation

While Quaternians define the rotational transformation with an angle around a specified axis, Euler rotation is defined by 3 Parameters (compare `Transform Node` x, y, z).


### Blending Quaternions with `slerp()`

### Sources /  further Reading:
- [[VEX for Algorithmic Design] E14 _ Quaternion Basics](https://www.youtube.com/watch?v=MYRtwY-DQV8)

### Related:
- [[notes/Matrix Operations |Matrix Operations]]