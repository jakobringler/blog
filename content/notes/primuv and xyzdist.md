---
title: "primuv & xyzdist"
tags:
- houdini
- vex
---

This is mostly a write up of different blog posts, tutorials, cg wiki's [Joy Of Vex 19](https://www.tokeru.com/cgwiki/index.php?title=JoyOfVex19) and Toadstorm's [The joy of xyzdist() and primuv()](https://www.toadstorm.com/blog/?p=465) for myself, so I recommend to check those out first!

### Usecases

- Stick things to geometry using VEX
- Smoothly sample / interpolate attribute values from the closest geometry (avoids flickering as closest point changes -> problem with `minpos()` / `nearpoint()`

### primuv() function

`primuv()` provides a way to smoothly interpolate an attribute between points on the target geometry.

![[notes/images/primuv_sample_ex.gif]]
In this example I sample the color and the position at the vector `uv` and apply it to a single point before copying a sphere to it.

// pointwrangle "sample_primuv":
```C
vector uv = chv("vector");
v@Cd = primuv(1, "Cd", 0, uv);
v@P = primuv(1, "P", 0, uv);
```

// vex definition
```C
<type> primuv(<geometry>geometry, string attribute_name, int prim_num, vector uvw)
```

If you give it a primid and a uv coordinate it returns the value of the specified attribute at the uv-interpolated position. UVs in this case are not typical uv coordinates used for texturing, but special **parametric UVs**.

### Interpolating quaternions (p@orient)

To correctly interpolate quaternions you need spherical interpolation. The `primuv` function only interpolates linearly. To counter this issue you can convert any quaternion on your reference geometry to a `matrix3` transform and then sample it, before converting it back to a quaternion on the target geometry.

This is basically the same as creating 3 vectors, which represent the quaternion, and interpolating those.

You should also use `polardecomp()` on your matrix to make sure it is orthonormal and avoid getting "a wonky quaternion".

the full code looks like this:

// point wrangle on reference geometry
```C
matrix3 m  = qconvert(p@orient);
```

// point wrangle on target geometry
```C
matrix3 interp_matrix = primuv(1, "m", prim_num, uv);
p@orient = quaternion(polardecomp(interp_matrix)); 
```

> [!quote] **Sources:**
> @Reinhold pointed this out on a houdini discord. Thanks!!

### Parametric UVs vs Regular UVs

While regular UVs usually span accross multiple primitives and map a 2D space to the 3D geometry, paramteric UVs are calculated automatically per primitive. They also don't exist as a `@uv` attribute.

Parametric UVs are a 0 to 1 gradient mapped across the prim. Think of a default STMap in Nuke where the x and y-axis resolution is fit to a 0-1 range and put into the red and green image channels.

![[notes/images/stmap_primuvs.png]]

You can visualize them using the scatter SOP. Just turn on `sourceprimuv` but change it to `Cd` in the `output` tab.

![[notes/images/primuvs_scatter.png]]

All the primitives (**NOT** volumes, vdbs, metaballs) have continuous parametric UVs:

![[notes/images/primitive_primuvs.png]]

### xyzdist() function

While the `minpos()` function can be used to find the closest point on a geometry, the `xyzdist()` function returns the `id` of the closest and the parametric `uv` coordinates of this primitive at which a point would be closest. This info can then be fed into the `primuv()` function.

// vex definition
```C
float  xyzdist(<geometry>geometry, string primgroup, vector origin, int &prim, vector &uv, float maxdist)
```

The `&` symbols mean that vex will write to the attributes you specify. This is necessary because the `xyzdist()` function can return more than one value at the same time and vex can't handle filling multiplte variables at once otherwise. It's similar to the `rotate` and `scale` matrix functions.

By default `xyzdist()` returns the distance to the closest "point" that lies on the surface of the target geometry. By using the additional `&attributes` we can get the id and uv information I mentioned before.

The usual setup  to query information by combining it with the `primuv()` function looks something like this:

```C
i@prim; 
v@uv;
f@dist;
 
@dist = xyzdist( 1, @P, @prim, @uv );
 
@P = primuv( 1, 'P', @prim, @uv );
```

Unlike `minpos()` you can read all sorts of attributes with this and not just `@P`.

### Make Stuff Stick to Animated Geo

![[notes/images/xyzdist_sticktogeo_ex.gif]]

To make this work we need to make sure to:
1. create a `p@orient` attribute that follows the animated geo correctly
2. freeze the animation before placing calculating `xyzdist()`

![[notes/images/sticktogeo_xyzdist.png]]

// pointwrangle "xyzdist"
```C
i@prim; 
v@uv;
f@dist;
 
@dist = xyzdist( 1, @P, @prim, @uv );
```

//pointwrangle "primuv"
```C
@P = primuv( 1, 'P', i@prim, @uv );
p@orient = primuv( 1, 'orient', i@prim, @uv );
```

The rest ist pretty much the same as described above. Calculate `xyzdist()`, use `primuv()` to fetch `@orient` from the animated geo, copy to points etc.

---

sources / further reading:
- [The joy of xyzdist() and primuv() - Toadstorm](https://www.toadstorm.com/blog/?p=465)
- [Joy Of Vex 19 - cgwiki](https://www.tokeru.com/cgwiki/index.php?title=JoyOfVex19) 
- [primuv - Houdini VEX DOCs - SideFX](https://www.sidefx.com/docs/houdini/vex/functions/primuv.html)
- [xyzdist - Houdini VEX DOCs - SideFX](https://www.sidefx.com/docs/houdini/vex/functions/xyzdist.html)


