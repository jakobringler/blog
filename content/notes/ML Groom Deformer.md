---
title: ML Groom Deformer
draft: true
tags:
---
This is a project I presented at the Houdini HIVE Horizon event SideFX hosted in Toronto. You can watch the full presentation [here](https://www.youtube.com/watch?v=oDTResIxPeQ). I'll go into more specific setup details in this text summary.

![[notes/images/previewimage.jpg]]
## General Idea
In essence you want to predict how fur deforms on a character/creature using an almost real-time machine learning model, instead of running expensive simulations/procedural setups. 

The setup is based on the[ ML Deformer example](https://www.sidefx.com/contentlibrary/ml-deformer-h205/) that was included in the content library with 20.5's release.
### The Problem
There's a couple of common issues when deforming hair naively/linearly. Especially around the joint areas there are lots of intersection issues, where hair above the joint pierces through the fur layer below (see second position in graphic below). What you ideally would want to see is a nice layering of guides and better volume preservation. This is computationally pretty expensive and requires a simulation and sometimes one or more post processing steps to get right. 

![[notes/images/ML-groom-input-target-comparsion.gif]]

left to right: Rig, Default Guide Deform, Ideal Deformation (Quasi-Static Simulation), Rest Guides, Displacement in Rest Space
### The Solution
What we want to do is predict this deformation that has to be applied to the guides based on the rig position using a ML model. The model learns to map from the rotation data of each rig joint to the approximated displacement of the guides for each pose..

To do this we need loads of examples that we can show the model to learn from. The general pipeline looks something like this:

![[notes/images/mlgroom_datageneration.png]]

1. Define joint limits on your rig (fixed values or automatically in a procedural fashion)
2. Generate a bunch of random poses based on these joint limits (think 3000 or more /you need good coverage)
3. Simulate a ground truth/good guide deformation for each pose
4. Collect all deformations and compress the data into a PCA subspace (conceptually the hard part)
5. Store the full preprocessed data set to disk
6. Train a ML model in TOPs (the easy part actually)
## Data Generation
To generate all the training data we need 2 things to start out with:
1. a rigged model of a character/creature (in this case the wolf)
2. a set of guides
### Joint Limits
This could be a set degree value on each joint (say 30) or better, if you have a couple motion clips for the rig, you can procedurally configure the joint limits based on the range of motion present in your clips. To do this use the `configure joint limits SOP`.
### Pose Randomization
In Houdini 20.5 a new node to streamline this process was introduced. The `ml pose generate SOP` takes existing joint limits and generates random poses in a gaussian distribution. This means there will be more normal poses than extreme ones, which is desirable to train a model that performs well in common positions.

![[notes/images/ML_groom_generated_poses.gif]]

The output is a bunch of posed packed rigs stacked on top of each other.
### Simulation Loop
In this stage we run over each of our generated poses using TOPs and generate/simulate a good guide deformation result accordingly. 

This step could be almost anything. There is just a few limitations:

The result has to be deterministic / procedural / quasi-static!

That means you can't have any inertia (think jiggle, overshoot, simulation goodness) and you should always get the same result when you rerun the same inputs. Also the inputs should influence the outputs in a smooth way. Inputs that are close to each other should generate outputs that are close to each other as well. So don't use a noise that generates wildly different results on inputs that are almost identical.

In this case I'm using a combination of a quasi-static tet simulation and a de-intersection pass, in which the guides are advected through a velocity field built from the hair and skin.

# more info on blending to pos

![[notes/images/animate_to_pos.gif|400]]
#### Quasi-Static Tet Simulation (Vellum)
To simulate the main part of the fur I build a tet cage from the guides to then simulate the volume of the fur and how it deforms. This is very useful to not have to deal with guide/guide collisions and ensures correct layering of fur by design.

![[notes/images/guides_to_tets.gif|400]]

To generate the tet cage I use some simple vdb operations: 

guides > VDB from particles > reshape dilate close erode >tet conform

# more info on sim in vellum

![[notes/images/simulate-tets-edited.gif|400]]

After applying the deformation with a point deform you get rid of all the jagged edges and most of the intersections

![[notes/images/pointdeform_to_tet_cage.gif|400]]
#### De-Intersection through Velocity Advection 
To clean up any left over guide intersections I advect the guides through a velocity field that is built from the normals of the skin mesh and the tangents of the guides themselves. Due to the nature of velocity fields the guides basically de-intersect themselves and the flow gets clean up a little on the way there. I first saw this technique in Animalogic's [talk](https://www.youtube.com/watch?v=NgOxluYHb54) about their automated CFX pipeline. 

![[notes/images/velocity_deintersect.gif|400]]

#### Ground Truth Examples

![[notes/images/generated-guidedeformation.gif|400]]
#### Calculate Rest Displacement

![[notes/images/rest_space_disp_extraction.gif]]
### Storing the Examples
## PCA Subspace Compression
After generating all of our pose displacements we bring in those points all together and calculate out PCA subspace.
### Creating the Subspace

### Calculating the Weights per Example

## Training

### ML Regression Train

### Wedging

## Inference

## Results

![[notes/images/runcycle_sideview_02.gif]]

---

sources / further reading:
- [PCA Shenanigans and How to ML | Jakob Ringler | Houdini Horizon](https://www.youtube.com/watch?v=oDTResIxPeQ)
- [KineFX - APEX Rigging and Skinning with ML Deformer | Wave VFX | FMX HIVE 2024](https://www.youtube.com/watch?v=rPdOjkEwxQM)
- [ML Groom Deformer - Houdini 20.5 - SideFX](https://www.sidefx.com/contentlibrary/ml-deformer-h205/)

