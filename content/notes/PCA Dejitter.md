---
title: Geometry Denoise with PCA
draft: true
tags:
  - houdini
  - math
---
This is one of the projects I presented at the Houdini HIVE Horizon event hosted by SideFX in Toronto. You can watch the full presentation [here](https://www.youtube.com/watch?v=oDTResIxPeQ). I'll go into more specific setup details in this text summary. You can find an almost identical version of the text (better formatting) on the SideFX tutorial page and download the project files from the content library.

If you haven't heard of [[notes/Principal Component Analysis|PCA]] yet, I recommend taking a look at the [[notes/Bounding Box Orientation on Arbitrary Point Clouds with PCA|Bounding Box Orientation]] example first.
## The Problem

Sometimes you get jittery geometry from faulty simulations or external capture processes and don't have time or the option to resim/recapture/do it the right way. We can then use [[notes/Principal Component Analysis|Principal Component Analysis]] to de-jitter geometry. 

![[notes/images/pca_cloth_dejitter_rendered_guideresult_v01.gif|500]]

Here we can see the jittery input on the left and the result of PCA filtering on the right.
## The Solution

The content library file looks pretty involved, but the actual important part is in the 3 network boxes in the middle (purple, green, other purple?). Those are also exactly the same setup. The only thing that changes is what you feed into it. More on that later!

![[notes/images/pca_dejitter_wholenetwork.png]]
### PCA Setup

![[notes/images/pca_setup_clean.png|500]]

### Why does this work

timeshift

`$F-(detail("../foreach_count5", "numiterations", 0)/2)+detail("../foreach_count5", "iteration", 0)`


![[notes/images/pca_stackedview_slide.jpg]]


![[notes/images/pca_reconstruction.png]]

![[notes/images/pca_blendshapesamples.png]]

## The Result
As you can see below, we got rid of most of the jitter. Depending on how aggressive we dial the settings in, we can remove even more.

![[notes/images/pca_cloth_dejitter_rendered_naive_v01.gif|500]]

There is one issue though. By removing all the small movements and quick direction changes we also de-jitter the animation itself, which you can especially tell when the model claps and the hands seemingly get stuck in the air. 
## Improvements

There are some workarounds that we can do to counteract that problem, but we would need a full rig to do so. This is external data and I only have alembic cashes of the character and the cloths. That is enough however to save us. 

I will first go over how this would ideally work when we would have the rig and demonstrate that on a simple example. 
### De-Jittering in Rest Space


### De-Jittering with a Guide

![[notes/images/dejitter_simple_compared.png]]

For the cloth simulation example that would look something like this:

![[notes/images/pca-cloth-dejitter-newclothes-03.gif]]
## Credits

Thanks to the amazing team at SideFX for all the help with this project!

Support on All Fronts: Fianna Wong
ML Tools Developer: Michiel Hagedoorn

---

sources / further reading:
- [Principal Component Analysis - Houdini DOCs](https://www.sidefx.com/docs/houdini/nodes/sop/pca.html)


