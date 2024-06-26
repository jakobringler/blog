---
title: "VELLUM"
tags:
- houdini
- FX
---

## Cloth

### Stiff & Crumply (Paper, Metal, etc.)

Key Parameters:
- Constraint Iterations (higher = stiffer)
- Plasticity (prevents unfolding)
- High Bend Stiffness
- Disable Compression Stiffness
- Enable Stiffness Dropoff (Decreasing)

Additional Tricks:
- Automatically group sharp corners and use the resulting 'crease' group in a subdivision node to maintain sharpness

### Intersections
When dealing with intersections on layered cloth the 'post collision passes' parameter can help. A good rule of thumb is setting them slightly above the number of expected layers (collisions with itself) of cloth.

![[notes/images/layersofcloth.png]]

In this example where two cloth folds form I would count 4 possible collisions and probably set 'post collision passes' to 5.
## Hair

### Collisions
To fix jittering and collision issues on hair roots the `disableexternal` attribute can be useful.

```C
if(@curveu == 0)
{
	i@disableexternal = 1;
}
```

similarly `i@disableself = 1` can be used to avoid collisions with itself.

### Stiff Hair (LowRes / HiRes Workflow)

To get anything rigid in Vellum you need loads of constraint iterations usually. For Hair you can sometimes get away with using a low resolution (sparsely resampled) version of the guides and simulate those. 

All you have to do is put some uvs on each hair and lookup the new position with the `primuv` function, which takes care of the interpolation of the hiRes guides.

// simulation guides (left) are resampled to about 1/5 of the actual point count

![[notes/images/lowResHiRes_hair.png]]

// both uvtexture nodes

![[notes/images/uvtexture_hair.png]]

// point wrangle

```C
v@P = primuv(1 ,"P", @primnum, v@uv);
```

## Fluids
### Droplets, Sheets & Tendrils
same rules as described in [[notes/FLIP#Droplets Sheets Tendrils |FLIP]] apply.


---

sources / further reading:
- [Houdini vellum edge threads details - Tutorial - Linus Rosenqvist](https://www.youtube.com/watch?v=3IidlkG-VmM)
- [Crumple Effects in Houdini Vellum using Bend Stiffness and Plasticity - regularmenthol](https://www.youtube.com/watch?v=64ujNBGQ7P8)
- [Houdini Quicktip: Vellum Creasing - Dave Stewart](https://vimeo.com/601670425)
- [disableexternal - sideFX Forum](https://www.sidefx.com/forum/topic/60379/?page=1)


