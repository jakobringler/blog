---
title: "Common Attributes"
draft: false
tags:
- houdini
---

This is my own extended & modified list of attributes based on SideFX DOCs, John Kunz' [VEX Attribute Glossary](https://wiki.johnkunz.com/index.php?title=VEX_Attribute_Glossary), Matt Estella's [VexCheatSheet](https://www.tokeru.com/cgwiki/VexCheatSheet) and others.

## Every Day Use

### Global Attributes
Available in most wrangles

```C
// Available in all SOP wrangles
i@Frame // current frame number => $F
f@Time // current time in seconds => $T
f@TimeInc // timestep currently being used for simulation or playback

// only in DOPs
i@SimFrame // integer simulation timestep number => $SF
f@SimTime // simulation time in seconds => $ST

// Available in Attribute Wrangle (point, vertex, primitive and detail)
v@P // position of the current element

i@ptnum // point number attached to the currently processed element
i@numpt // total number of points in the geometry
i@vtxnum // linear number of the currently processed vertex
i@numvtx // number of vertices in the primitive of the currently processed element
i@primnum // primitive number attached to the currently processed element
i@numprim // total number of primitives in the geometry

// useful: elemnum and numelem morph into whatever wrangle you are in
i@elemnum // index number of the currently processed element
i@numelem // total number of elements being processed

// Available in Volume Wrangle
v@P // position of the current voxel
f@density // density value at the current voxel location
i@resx, i@resy, i@resz // resolution
v@center // center of the volume bbox
v@dPdx, v@dPdy, v@dPdz // change in position that occurs in the x, y, and z voxel indices
i@ix, i@iy, i@iz // voxel indices (only dense volumes / non-VDBs ) range from 0 to resolution-1

// Available in Heightfield (Volume) Wrangle
f@height // height at xy coordinate
f@mask // mask at xy coordinate
```

### Geometry Attributes
stuff that Houdini knows, expects and casts automatically to the correct type

```C
// Int
@id // unique number > temporal coherent
@piece // like id but used more in rbd context

// Float
@pscale // particle radius size / uniform scale / set display particles as 'Discs' to visualize.
@width // thickness of curves.  Enable 'Shade Open Curves In Viewport' on the object node to visualize
@Alpha // alpha transparency override. Used by viewport to set the alpha of OpenGL geometry
@Pw // spline weight

// Vector3
@P // point position
@Cd // diffuse color override
@N // surface or curve normal. Houdini will compute the normal if this attribute does not exist
@rest // Used by procedural patterns and textures to stick on deforming and animated surfaces. Stores the P of a rest position
@uv // UV texture coordinates (point/vertex)
@v // point velocity. can be used for motionblur calculation
@w // angular velocity used for rbd generally
@force // used in simulation for many solvers that don't like you to edit v directly, but v will be calculated after updating force

// Vector4
@orient // local orientation of a point (represented as a quaternion)

// String
@name // unique name identifying which primitives belong to which piece 
@instance // path of an object node to be instanced at render time
@shop_materialpath // material assignment pre primitive
@path // used to rebuild alembic hierarchy
```

### Instancing / Copy Attributes

all the things the copy to points SOP understands:

```C
// Float
f@pscale // size of a copy

// Vector3
v@P // position of a copy
v@Cd // color
v@N // used to orient Z axis of the copy towards
v@up // up direction for local space, typically {0, 1, 0}
v@trans //translation of a copy
v@scale // scale of a copy
v@pivot // local pivot of a copy
v@v // velocity for motionblur, used as +Z if no orient or N is present

// Vector4
p@orient // local orientation of the point (quaternion)
p@rot // additional rotation. applied after orient, N, and up

// String
s@shop_materialpath // shader assignment
s@material_override // dict mapping parameter names to values to override material properties
s@instance
s@instancefile // file path to geometry to instance.
s@instancepath // geometry to instance. file on disk or an op: path

// Matrix 3x3 or 4x4
3@transform // transformation matrix (rotation, scale, and shear) overriding everything except translations from P, pivot, and trans
4@transform // transformation matrix (translation, rotation, scale, and shear
```

## Context Specific

### POP Attributes

```C
```

### Grain Attributes

```C
```

### Packed RBD Attributes

```C
```

### RBD Constraint Attributes

```C
```

### FLIP Attributes

```C
```

### Vellum Point Attributes

```C
```

### Vellum Constraint Attributes

```C
```

### KineFX

```C
s@name // joint name attribute
3@transform // world space 3×3 transform of the point (rotation, scale, and shear)
4@localtransform // transform of the point relative to its parent
i@scaleinheritance // determines how a point inherits the local scale from its parent
```

## Niche Stuff

### GL Viewport Attributes
// detail wrangle

```C
i@gl_wireframe // if set to 1: geometry will always appear as wireframe in viewport 
// if set to -1: geometry will always appear shaded in viewport -> allows for guide geometry to be drawn shaded
i@gl_lit // if set to 0: geometry will always appear without lighting 
i@gl_showallpoints // if set to 1: renders all points as sprites even if they are connected to geometry
i@gl_spherepoints // if set to 1: render points as spheres
i@gl_xray // if set to 1: draw geo in xray mode 
f@vm_cuspangle // controls cusp angle to generate normals when the geometry doesn't have any
s@gl_spritetex // custom sprite texture for points
i@gl_spriteblend // if set to 0: no depth sorting or blending
f@gl_spritecutoff // discards pixels with alpha value below this 

v@Cd             
f@Alpha 
v@N 
f@width // curve width > `Shade Open Curves In Viewport` has to be turned on on the geo node
f@pscale // if no pscale exists the viewport defaults to 1.0, while Mantra defaults to 0.1

i@group__3d_hidden_primitives  // add primitives to this group to hide them from the 3D viewport

f@intrinsic:volumevisualdensity // primitive intrinsic attribute controlling the opacity of volumes
f@volvis_shadowscale // detail attribute controlling the shadow strength for volumes
```

### Intrinsic Attributes

// primitive wrangle

```C
// Primitive Intrinsics
float primbounds[] = prim(0, "intrinsic:bounds", @primnum);
float primarea = prim(0, "intrinsic:measuredarea", @primnum);
float memusage = prim(0, "intrinsic:memoryusage", @primnum);
```

// any wrangle

```C
// Detail Intrinsics
int ptcount = detail(0, "intrinsic:pointcount", 0);
float bounds[] = detail(0, "intrinsic:bounds", 0);

```

> [!check] **Bounds:**
> 
> Bounding box intrinsic attributes, like `intrinsic:bounds` or `intrinsic:packedbounds` are returned in (xmin, xmax, ymin, ymax, zmin, zmax) order.

You can set some intrinsic attributes in wrangles:

// primitive wrangle

```C
setprimintrinsic(0, "pointinstancetransform", i@primnum, 1, "set");
```

This enables the use of the standard instancing attributes to transform any packed pieces (e.g. pscale, orient, ... ) further down stream **after setting it up!**

![[notes/images/pointinstancetransform_intrinsic.png]]

---

sources / further reading:
- [Copying and instancing point attributes - Houdini DOCs](https://www.sidefx.com/docs/houdini/copy/instanceattrs.html)
- [Viewport display attributes - Houdini DOCs](https://www.sidefx.com/docs/houdini/model/attributes.html)
- [VexCheatSheet - cgwiki](https://tokeru.com/cgwiki/VexCheatSheet)
- [VEX Attribute Glossary - John Kunz](https://wiki.johnkunz.com/index.php?title=VEX_Attribute_Glossary)

