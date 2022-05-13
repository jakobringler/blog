---
title: "Abusing Heightfields as Fast Image Canvas"
tags:
- houdini
- machinelearning
- python
- pytorch
---

### The Problem

A big part of getting a machine learning model to run is to prepare and 
 import the data in the right _shape_. Usually this shape will be some n-dimensional [[notes/Tensors |Tensor]]. 

When working with 2D image data the files are read from disk. There are tons of tools and functions in most of the DL Frameworks that do just that. In Houdini however, the data at hand is most likely not in a format (.jpg .png etc.) that can be extracted easily by a premade data loading solution.

To extract the data somewhat _liveish_ without having to write anything to disk and read it back in another step, a function that reads directly from houdini geometry is more efficient.

Unfortunately storing image data on 3D geometry isn't very efficient. 


### The Solution

**Heightfields**. or 2D-Volumes (weird name) 

They have a grid-like topology and only store a single value per voxel instead of unnecessary connectivity information or other  data. 

Houdini ships with a python function to extract voxel data quickly, which allows us to convert the 2D information into the necessary shape.

```Python 
# this runs in a python SOP
import hou
import numpy as np

node = hou.pwd()
geo = node.geometry()

W = 128 # image width
D = W

canvas = geo.prim(0) # reads the 0th primitive 

canvasVoxels = canvas.allVoxels() # reads all voxel values as one big array [1, 2, ... , n]

input = np.asarray(canvas, dtype=np.float64)
input = input.reshape(W,D) # creates a tensor of shape [128,128,1]
```


