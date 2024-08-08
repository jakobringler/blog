---
title: Storing Grooms in Volumes
draft: false
tags:
  - houdini
  - machinelearning
---

A common problem with _most_ machine learning applications is that you need your data to be of the same size. That means the input or output [[notes/Tensors|Tensors]] have to have the same shape across all of the examples. 

That's why images or volumes or other grid like data structures lend themselves well to machine learning purposes.

I was trying to find a way to express hair/fur grooms in a way that doesn't change the shape of the data, if you change the hair count or segment length of the groom. I came across this [paper](https://mlchai.com/files/lyu2020real.pdf) on neural interpolation for real-time hair simulation, which had some great ideas.

The main one is to store the groom in a volume. 

You can use the UVs of your mesh geometry to specify a new coordinate system consisting of the u and the v axis and use the `curveu` attribute on each hair as the third axis to move the groom to a new space, which by the nature of UVs becomes cuboid and can be easily rasterized into a volume. 

![[notes/images/groom2uvw.gif]]

The rasterization part acts like a filter and you lose some information along the way, but I found a volume of 512 x 512 x 20 to already be sufficient in quality for most usecases.

![[notes/images/groom2volume.png]]

// point wrangle move_to_tensor_space

```C
float voxelsize = volumevoxeldiameter(1, 0)/2;
vector dims = primintrinsic(1, "activevoxeldimensions", 0);
float voxelcount = dims.z;
v@restP = v@P;
v@P = set(@uv.x, @uv.y, @curveu*voxelsize*voxelcount);
```

// point wrangle rebuild

```C
float voxelsize = volumevoxeldiameter(2, 0)/2;
vector dims = primintrinsic(2, "activevoxeldimensions", 0);
float voxelcount = dims.z;
vector readPos = set(@uv.x, @uv.y, @curveu*voxelsize*voxelcount);
v@P = volumesamplev(1, 0, readPos);
```

Now you can learn stuff on arbitrary grooms! long, short, thick or thin :) 

---

sources / further reading:
- [Real-Time Hair Simulation with Neural Interpolation - Qing Lyu et al.](https://mlchai.com/files/lyu2020real.pdf)

