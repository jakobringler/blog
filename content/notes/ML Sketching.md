---
title: ML Sketching
draft: true
tags:
  - houdini
  - machinelearning
---
This is one of the projects I presented at the Houdini HIVE Horizon event hosted by SideFX in Toronto. You can watch the full presentation [here](https://www.youtube.com/watch?v=oDTResIxPeQ). I'll go into more specific setup details in this text summary. You can find an almost identical version of the text (better formatting) on the SideFX tutorial page and download the project files from the content library.

![[notes/images/drawing-demo.gif]]

The project builds on top of the [[notes/Tower Sketcher|Tower Sketcher]] demo I created in 2022 for my [[notes/ML Castles|ML Castles]] bachelor's project by adding [[notes/Principal Component Analysis|PCA]] into the mix. Also it is now running end to end inside of Houdini without having to write any code yourself, which makes it much more approachable!
## General Idea
The high level goal is pretty clear: control geometry generation using a canvas style drawing UI as user input. The main problem here is that it is super hard (to impossible) to predict proper topology or even meshes at all. Neural networks like grid-like data topology as inputs/outputs.

Shapes like arrays, images or voxel volumes work well. Any one of these with a fixed size can be used as input or output for training relatively easily. Houdini's procedural workflows come in very handy here: We can create an HDA or other type of generator setup that builds our desired object. What we then want the ML model to learn is to map from a silhouette drawing to the parameters that drive the generator to produce the correct object procedurally.

float image grid --> float array

![[notes/images/mlsketch_architecture_slide.png]]

Doing so, we avoid all sorts of pitfalls and pain trying to predict geometry. The architecture comes with different pros and cons, which I will explain later. However, most of them are rather desirable, such as having clear constraints on what the model can output and not being too limited by the small resolution of the canvas:
- no matter what input is given the generator will always generate a somewhat sensible result 
- the generator can generate model details, that don't need to be reflected in the input drawing
## Generator
in this case compiled block for convenience and reusability reasons

could be hda or anything

gets controlled by parm dictionary attribute that gets read and distributed inside the block

all values are -1 to 1 range which is a good normalization and fits the tanh() activation function that gets used inside the network

parm inputs are remapped using sigmoid and cubic functions to control the distribution of random values when randomizing the generator later:

- cubic -> more values around 0 
- sigmoid -> more values on the extremes (-1, 1)

![[notes/images/mlsketch_generator_slide.png]]

![[notes/images/mlsketch_unpackfolders_slide.png]]
## Data Generation

Compared to the [[notes/ML Groom Deformer|ML Groom Deformer]] demo, where the data generation is automated using TOPs, this time everything runs in a simple for each loop. This makes more sense because each example only takes a second or less to generate and spinning up entire Houdini instances in TOPs would create a bottle neck. Depending on the generation time and amount of examples needed it might make sense to implement a mix of both approaches: 

Generate a bunch of examples in one workitem, but have multiple instances run at same time

Because the generator is just the compiled block we need to randomize the input dictionary. This is a super simple detail expression that randomizes each value based on the current iteration. Make sure to give a different offset/seed to each otherwise you get the same value in each parameter.

The `invoke compiled block SOP` then runs the generator and spits out our merged geometries, which we can access using the `unpack folder SOP` as mentioned above. The rest is just some data wrangling and rasterizing the outline on the image plane before storing it together with the parameter value list as a ml example.

![[notes/images/mlsketch_datageneration_slide.png]]
## PCA Subspace Compression

what if we do pca on the inputs?

![[notes/images/bottle_pca_slide.png]]

Reconstruction

![[notes/images/bottle_pcareconst_slide.png]]

meh

What if SDF

![[notes/images/bottle_tosdf_slide.png]]

SDF reconstructed

![[notes/images/bottle_sdfreconstructed_slide.png]]

PCA Components

![[notes/images/pca_components.jpg]]

User Input (gets turned into an sdf under the hood)

![[notes/images/drawn_input.png]]

Weighted PCA Components based on Inputs

![[notes/images/pca_components_weighted.jpg]]

Adding them all together

![[notes/images/weighted-components-reconstructe.gif]]
## Data Overview

![[notes/images/training-data-02.gif]]
## Training
very simple straight forward
### Hyperparameter Tuning with Wedges
some wedging
nothing special
## Inference
everything backwards
### Performance
pretty fast, decent accuracy

always some output > constrained by generator
### Constraints
## What if? Predicting Full SDFs

### Having 2 Generators

## Inference

### Performance


---

sources / further reading:
- [PCA Shenanigans and How to ML | Jakob Ringler | Houdini Horizon](https://www.youtube.com/watch?v=oDTResIxPeQ)

