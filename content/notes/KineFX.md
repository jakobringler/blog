---
title: "KineFX"
draft: false
tags:
- houdini
---

## Rig Attribute Wrangles

### Basics

We can use any kind of curve and transform it like a parent hierarchy chain using the `Rig Attribute Wrangle`!

![[notes/images/roll_up_kinefx.gif]]

// rigattribwrangle

```C
prerotate(4@localtransform, chf("amount") * @curveu, {1,0,0});
```

The wrangle automatically creates a `transform` and `localtransform` matrix on each point to apply all transformations.

Usually you would do this beforehand to better control the orientations. A good way is using the `orientalongcurve` SOP which has a 3x3 transform export option in combination with a `rigdoctor` node.

### Secondary Motion with CHOPs

The issue with the default secondary motion SOP is that you can only have a single driver point for now (H19.5). If you want to control many different joints with different drivers without setting up a secondary motion SOP for each you are out of luck. 

I had to build a jiggle effect for an unfolding tree with a lot of branches where I would have needed this functionality.

As a workaround I animated an `angle` attribute and jiggled it using CHOPs before deforming my rig with the `prerotate` & `prescale` function. 

![[notes/images/CHOP_rig_jiggle.png]]

// rotate_rig wrangle
 
```C
prerotate(4@localtransform, @angle, axis);
prescale(4@localtransform, scale_vector);
```

I used the `AttribChop` SOP from Nick Taylor's [aeLib](https://github.com/Aeoll/Aelib) to do the jiggle setup more conveniently. Highly recommended HDA collection!

It gives quite convincing results without having to sim anything.

![[notes/images/Tree_Chop_SecondaryMotion.gif]]

## Deformation in Rest Space

Sometimes you want to see/work with a simulation/deformation result in rest space. To do this you need to create a local coordinate system for every point and then deform that with geometry. 

![[notes/images/restspacedeformation.gif]]

> [!quote] **Sources:**
> 
>This setup was shown to me by Michiel Hagedoorn @ SideFX, while I was working on the [[notes/ML Groom Deformer|ML Groom Deformer]] project and it comes in handy every once in a while!

![[notes/images/extract_deformation_restspace.png|400]]

// point wrangle "apply_local_coords"

```C
v@axis_x = set(1, 0, 0);
v@axis_y = set(0, 1, 0);
v@axis_z = set(0, 0, 1);

setattribtypeinfo(geoself(), 'point', 'axis_x', 'vector');
setattribtypeinfo(geoself(), 'point', 'axis_y', 'vector');
setattribtypeinfo(geoself(), 'point', 'axis_z', 'vector');
```

You can then introduce some sort of deformation that goes beyond the linear blend skinning one. In this case I'm using the wrinkle deformer, but this could be anything really (Vellum Sim etc.).

// point wrangle "extract_local_disp"

```C
matrix3 local_from_global = invert( set( v@axis_x, v@axis_y, v@axis_z ) );
vector global_delta = point(1,"P",@ptnum) - v@P;
vector local_delta = global_delta * local_from_global;

v@P = local_delta;
```

// point wrangle "apply_disp"

```C
v@P += point(1, "P", @ptnum);
```

The resulting geometry has all the deformation data on it with the input animation being removed. You can easily apply any effects and processing now and use a default bonedeform to move it back to the correct pose after.
##### Download: [File](https://github.com/jakobringler/blog/tree/hugo/content/notes/sharedfiles/deformation_in_restspace.hip)
## Misc

### Display Rig Controls on HDA Viewer State

To expose the control rig on an HDA you have to define the "Default State" parameter in the "Node" tab of the Operator Type Properties. 

For kineFX controls this should be `kinefx__rigpose`.

You also have to promote the parameters you want to control with the HDA.

---

sources / further reading:
- [Flower Garden | Carl Krause | Paris HIVE 2023 - SideFX](https://www.youtube.com/watch?v=9bsb-WBGPTc)
- [KineFX Rigging | Fur Dude | Part 9 | Add More Control Joints - SideFX](https://www.youtube.com/watch?v=3Ssa9xXnx9E)

