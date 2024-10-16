---
title: Geometry Denoise with PCA
draft: true
tags:
  - houdini
  - math
---
This is one of the projects I presented at the Houdini HIVE Horizon event hosted by SideFX in Toronto. You can watch the full presentation [here](https://www.youtube.com/watch?v=oDTResIxPeQ). I'll go into more specific setup details in this text summary. You can find an almost identical version of the text (better formatting) on the SideFX tutorial page and download the project files from the content library.

If you haven't heard of [[notes/Principal Component Analysis|PCA]] yet, I recommend taking a look at the article on [[notes/Principal Component Analysis|article on it]] and the [[notes/Bounding Box Orientation on Arbitrary Point Clouds with PCA|Bounding Box Orientation]] example first.
## The Problem

Sometimes you get jittery geometry from faulty simulations or external capture processes and don't have time or the option to resim/recapture/do it the right way. We can then use [[notes/Principal Component Analysis|Principal Component Analysis]] to de-jitter geometry. 

![[notes/images/pca_cloth_dejitter_rendered_guideresult_v01.gif|500]]

Here we can see the jittery input on the left and the result of PCA filtering on the right.
## The Solution

The content library file looks pretty involved, but the actual important part is in the 3 network boxes in the middle (purple, green, other purple?). Those are also exactly the same setup. The only thing that changes is what you feed into it. More on that later!

![[notes/images/pca_dejitter_wholenetwork.png]]
### PCA Setup

![[notes/images/pca_setup_clean.png|500]]
#### Merging Frames
We need to merge all the frames we want to look at. Ideally the number of frames should be uneven (say 7: 3 past frame, the current one and 3 future frames). That we get an equal amount of past and future frames. The output of the for loop is the different geometries stacked on top of each other.

![[notes/images/pca_stackedview_slide.jpg]]

The timeshift uses this expression to access the frames based on the iteration count we set.

```C
$F-(detail("../foreach_count5", "numiterations", 0)/2)+detail("../foreach_count5", "iteration", 0)
```
#### Computing Components
Next we let PCA compute the components. In the [[notes/Bounding Box Orientation on Arbitrary Point Clouds with PCA|Bounding Box Orientation]] Example each sample was a single point in 3D space. Now one sample is the entire geometry of a single frame. So in this case we have 7 samples (7 frames). We give PCA a giant list of all the point position vectors and let it figure out what is the most important bit of information across all the samples given. 

![[notes/images/pca_blendshapesamples.png]]

Because the geometries we feed into PCA don't have id or piece attributes or any other information on which point belongs to which "pose"/frame, we need to tell the PCA node how big each sample is:

// Expression on "Points per Sample" Parameter

```C
npoints(0)/ch("../foreach_end3/iterations")
```

 PCA then "invents" one or more components that can be thought of as blendshapes. Creating one component creates one blendshape that best fits all of the input samples. Creating more gives you multiple blendshapes that you can later mix and match to rebuild a specific pose.
#### Projecting and Reconstructing

// point wrangle "weightdecay"

```C
float range = 1-(float)@ptnum/@numpt;
float weightdecay = chramp("decay", range);
@weight *= weightdecay;
```

![[notes/images/pca_reconstruction.png]]
### Why does this work
The approach works because the components PCA generates are ordered by importance. Or in other words influence they have on the end result. We can abuse this to filter out less important information, by discarding higher order components. The noise/jitter that is present in the input data changes a lot frame by frame and isn't really important for the general shape. The assumption is that this less important information will be stored in higher order components.
Reconstructing the geometry without those removes the fine grained movements, noise and jitter.
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


