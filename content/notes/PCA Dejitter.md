---
title: Geometry Denoise with PCA
draft: true
tags:
  - houdini
  - math
---
### Denoising

In this example I added random noise spikes to the left Craig. The middle one is the "filtered" result and the right one is the original animation.

![[notes/images/PCA_geo_noise_removal.gif]]

It's not perfect, but the idea should work pretty well for less extreme noise/jitter.
### Setup

The setup is slightly more involved than before.

![[notes/images/PCA_geo_denoise_setup.png]]

timeoffset expression: `$F-(detail("../foreach_count2", "numiterations", 0)/2)+detail("../foreach_count2", "iteration", 0)+1`

compute_components expression: `npoints(0)/ch("../foreach_end2/iterations")`

compute_weights expression: `npoints(0)`
![[notes/images/geo_denoise_pca2.png]]

reconstruct expression: `ch("../compute_components/stride")`
![[notes/images/geo_denoise_pca3.png]]
### How it works

1. Use a for loop set to `feedback merge` and bring in multiple geometry frames at one point in time. The iteration number of the for loop defines how many frames of past and future data will be fed to the PCA node.
2. the first PCA node analysis the data and spits out a number of components. (typically 1 is enough)
3. The other two PCA nodes rebuild (project and reconstruct) the original point counts in the right position in space
4. The only thing left to do is to copy the new `P` values to the geometry in a simple wrangle and recalculate the normals

---

sources / further reading:
- [Principal Component Analysis - Houdini DOCs](https://www.sidefx.com/docs/houdini/nodes/sop/pca.html)


