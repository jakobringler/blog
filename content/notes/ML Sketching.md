---
title: ML Sketching
draft: true
tags:
  - houdini
  - machinelearning
---
This is a project I presented at the Houdini HIVE Horizon event hosted by SideFX in Toronto. You can watch the full presentation [here](https://www.youtube.com/watch?v=oDTResIxPeQ). I'll go into more specific setup details in this text summary. You can find an almost identical version of the text (better formatting) on the SideFX tutorial page and download the project files from the content library.

![[notes/images/drawing-demo.gif]]

The project builds on top of the [[notes/Tower Sketcher|Tower Sketcher]] demo I created in 2022 for my [[notes/ML Castles|ML Castles]] bachelor's project by adding [[notes/Principal Component Analysis|PCA]] into the mix. Also it is now running end to end inside of Houdini without having to write any code yourself, which makes it much more approachable!
## General Idea


![[notes/images/mlsketch_architecture_slide.png]]
## Generator

![[notes/images/mlsketch_generator_slide.png]]
## Data Generation

Compared to the [[notes/ML Groom Deformer|ML Groom Deformer]] demo, where the data generation is automated using TOPs, this time everything runs in a simple for each loop. This makes more sense because each example only takes a second or less to generate and spinning up entire Houdini instances in TOPs would create a bottle neck. Depending on the generation time and amount of examples needed it might make sense to implement a mix of both approaches: 

Generate a bunch of examples in one workitem, but have multiple instances run at same time

Because the generator is just the compiled block we need to randomize the input dictionary. This is a super simple detail expression that randomizes each value based on the current iteration. Make sure to give a different offset/seed to each otherwise you get the same value in each parameter.

The `invoke compiled block SOP` then runs the generator and spits out our merged geometries, which we can access using the `unpack folder SOP` as mentioned above. The rest is just some data wrangling and rasterizing the outline on the image plane before storing it together with the parameter value list as a ml example.

![[notes/images/mlsketch_datageneration_slide.png]]
## PCA Subspace Compression



## Data Overview

![[notes/images/training-data-02.gif]]
## Training

### Hyperparameter Tuning with Wedges
## Inference

### Performance

### Constraints
## What if? Predicting Full SDFs

### Having 2 Generators

## Inference

### Performance


---

sources / further reading:
- [PCA Shenanigans and How to ML | Jakob Ringler | Houdini Horizon](https://www.youtube.com/watch?v=oDTResIxPeQ)

