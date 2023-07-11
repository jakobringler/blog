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

## Misc

### Display Rig Controls on HDA Viewer State

To expose the control rig on an HDA you have to define the "Default State" parameter in the "Node" tab of the Operator Type Properties. 

For kineFX controls this should be `kinefx__rigpose`.

You also have to promote the parameters you want to control with the HDA.

---

sources / further reading:
- [Flower Garden | Carl Krause | Paris HIVE 2023 - SideFX](https://www.youtube.com/watch?v=9bsb-WBGPTc)
- [KineFX Rigging | Fur Dude | Part 9 | Add More Control Joints - SideFX](https://www.youtube.com/watch?v=3Ssa9xXnx9E)

