---
title: "Abusing Heightfields as Fast Image Canvas"
tags:
- houdini
- machinelearning
- python
- pytorch
---

### The Problem

a big part of getting a machine learning model to run is to prepare and 
 import the data in the right _shape_. Usually this shape will be some n-dimensional [[notes/Tensors |Tensor]]. 
Usually images are read from disk. There are tons of tools and functions in most of the DL Frameworks that do just that. In Houdini however, the data at hand is most likely not in a format (.jpg .png etc.) that can be extracted easily by a premade data loading solution.
To extract the data somewhat _liveish_ without having to write anything to disk and read it back in another step a function that reads directly from houdini geometry is more efficient.
Unfortunately storing image data on 3D geometry isn't very efficient.


### The Solution

**Heightfields**. or 2D-Volumes (weird name) 

They have a grid-like topology and only store a single value per voxel instead of unnecessary connectivity information or other  data. 

Houdini ships with a python function to extract voxel data quickly, which allows us to convert the 2D information into our 


