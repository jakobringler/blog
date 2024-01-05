---
title: Volumes
draft: false
tags:
  - houdini
---
## Inefficient Volumes

The following are my personal notes on Lewis Taylor's fantastic [SYDHUG Talk](https://vimeo.com/894363970). Definitely worth watching! There is also a download link for all the demo scenes in the video description.
### Impacts of Inefficient Volumes

- Slower simulation time
	- balance between needed fidelity and simulation time
- Disk storage
	- not as cheap as commonly assumed
	- impacts all departments
- Render times
	- not only dependent on voxel resolution
	- also has to load the cache
	- especially for BG elements the overhead to get the files is huge even though the pure rendering might be fast
- Server IO
	- not infinite
	- more people working at the same time can easily lead to bandwidth issues
- Slow to iterate
- Screwing yourself and your fellow Artist's out of the resource of TIME

### How to reduce Volumetric Data

- Voxel Resolution - how much do you need for the given Camera/render res
- Bit depth - 32 vs 16 - when does it matter? Can you use 8?
	- velocity and even density can be 16 bit in many cases
	- color field can easily be just 8 bit in most cases
- Culling fields - remove data in fields that aren't needed
	- delete voxel data in certain areas
	- cull velocity by density > if there is no density you won't need any velocity most of the time
	- can take 20%+ of the cache size
- Resampling - reducing voxel res per field - it doesn't all need to be the same
	- density, flame and temperature fields should be better left as they are
	- velocity on the other hand can be resampled to half or sometimes even quarter the res as it is only used to smear the other fields in the motion blur
	- can remove 65-70% of the cache size
	- velocity overhead is usually more than all the other fields combined (vector3)
- Frustum based rasterization - a camera based alternative to Cartesian grids
	- added in Houdini 20
	- Camera oriented voxel grid

### Frustum Volumes

Instead of orienting voxels along the xyz axis the camera frustum is used to rasterize the voxels to the cameras 2D plane x and y axis as well as the z depth.

##### PROS
- This can, depending on the simulation, divide your cache size by 10 or more!
- Especially useful for particle type simulations like whitewater where stuff trails or comes from far away close to the camera. Essentially every shot, where volume travels a fair amount of z depth will profit most from this approach.

##### CONS
- cannot be easily reused as it is locked to the camera perspective. Sometimes more caches for different cameras are still smaller than one big cartesian rasterized cache.
- In practice this means you have to rebuild your shaders to match the look of the cartesian rasterized volume 1:1, but it's not too bad.
- can be problematic when you expect shadows to be cast from a volume outside of your frustum. The lighting may look weird in that case

### Volume Compression

"it really really adds up"

Lewis compared 3 vdbs:

1. uncompressed
	32 bit 
	no field culling
	no field resampling
	**34** mb

2. compressed but still cartesian
	16 bit
	field culling - vel
	field resample - 2x vel
	**3.2** mb

3. compressed + frustum cull
	16 bit
	field culling - vel
	frustum cull in sim
	field resample - 2x vel
	**1.8** mb

### Other Useful Information
##### How many Voxels do you need per Pixel?

Usually you are good to go with 1/2.5 voxels per pixel. 

##### Frustum Rasterizing vs Frustum Culling SOPs vs Frustum Culling DOPs

1. Rasterizing

You need the `VDB Rasterize Frustum` SOP and a camera frustum VDB.
- Z  Scale on the VDB node controls the amount of voxels in camera depth
- It makes sense to bake in the motion blur using the according parameters on the vdb rasterize node

![[notes/images/furstumcull_node.PNG]]

2. Culling SOPs

If you just want to cut off anything outside the cameras frustum you can use the `VDB Clip` SOP.

3. Culling DOPs

To cull right inside the simulation you can use the following snippet inside a `Gas Field Wrangle` node piped into the advection output of the pyro solver.

// gas field wrangle

```C
// by Lewis Taylor

string cam = chsop("cam");
vector ndcP = toNDC(cam,@P);
//vector camP = ptransform("space:camera",@P); //non perspective space if needed

float mult = 1;

if(chi('cull_x_negative')) mult *= ndcP.x > -ch('cam_x_neg') ;
if(chi('cull_x_positive')) mult *= ndcP.x < 1 + ch('cam_x_pos') ;
if(chi('cull_y_negative')) mult *= ndcP.y > -ch('cam_y_neg') ;
if(chi('cull_y_positive')) mult *= ndcP.y < 1 + ch('cam_y_pos') ;
if(chi('cull_z_negative')) mult *= ndcP.z < ch('cam_z_neg') ;
if(chi('cull_z_positive')) mult *= ndcP.z > -ch('cam_z_pos') ;


//foreach( int i; string field; split(chs('fields'))){} //maybe loop over given fields, usually density is enough
f@density *= mult;
f@temperature *= mult;
f@flame *= mult;
@Cd *= mult;
```


---

sources / further reading:
- [SYDHUG_volumes - Lewis Taylor](https://vimeo.com/894363970)
