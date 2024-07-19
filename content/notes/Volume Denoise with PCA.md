---
title: Volume Denoise with PCA
draft: false
tags:
  - houdini
  - math
---
It's possible to use [[notes/Principal Component Analysis |Principal Component Analysis]] to analyze a sequence of volume data and denoise or deflicker it and even fill in missing frames. 
### Denoising

![[notes/images/PCA_noise_removal.gif]]
### Deflickering

![[notes/images/PCA_flicker_removal.gif]]
### Filling in Missing Frames

![[notes/images/PCA_blip_removal.gif]]

### Setup

The setup is pretty straight forward:

![[notes/images/PCA_denoise_setup.png]]

timeoffset expression: `$F-(detail("../foreach_count1", "numiterations", 0)/2)+detail("../foreach_count1", "iteration", 0)+1`

### How it works

1. Use a for loop set to `feedback merge` and bring in multiple **volume** ( ! not VDB) frames at one point in time. The iteration number of the for loop defines how many frames of past and future data will be fed to the PCA node.
2. for the PCA node parms refer to the screenshot
3. This gives you two volumes. You only need the one with id=0 so you can blast 1

The setup works for all three cases and depending on the amount of noise/flicker you can get away with 2-4 steps and maintain more volume detail. This whole thing gets pretty slow when combining highres volumes and high iteration settings. You've been warned. 

### Why it works

[[notes/Principal Component Analysis |Principal Component Analysis]] can be used to extract the most "meaningful" part of any given data. PCA gives you a number of components that are ordered in importance/"impact" on the full data. Because the noise changes from frame to frame while the big volume shapes only change relatively slowly, it can be filtered relatively easily by deleting higher order components. 

The same technique can be used on [[notes/Geometry Denoise with PCA |geometry]] in a similar setup,

---

sources / further reading:
- [Principal Component Analysis - Houdini DOCs](https://www.sidefx.com/docs/houdini/nodes/sop/pca.html)

