---
title: "ML Castles"
draft: false
tags:
- houdini
- machinelearning
- pytorch
- project
---

## Machine Learning Supported Procedural Castles
is a project I did for my bachelor thesis. The goal was to find out how machine learning could support procedural content genertation.
I created a procedural castle "generator", which is enhanced by two types of ML systems.

![[notes/images/CastleDemo.gif]]

The project files and code can be found on GitHub: [H_ML_Castles](https://github.com/jakobringler/H_ML_Castles)

### 1. Training a Neural Net to Understand my Sketches

![[notes/images/TowerSketcher.gif]]

more details can be found under: [[notes/Tower Sketcher |Tower Sketcher]]

### 2. Predicting Heightfield Data using cGANs

This Idea isn't anything new and was already shown in this [SideFX demo](https://www.sidefx.com/tutorials/machine-learning-data-preparation/). Since heightfields are essentially images (2D arrays) you can use conditional GANs to translate between one image to another.

You can for example translate height data from a base terrain to an eroded version of that terrain, like shown in the SideFX demo.

![[notes/images/ErosionPrediction.gif]]

In the same way you can then use the erosion data (height, water, sediment etc.) to predict where certain types of vegetation should appear. The model spits out different colored masks, which are used to scatter points for different plant or tree models.

![[notes/images/VegetationDemo.gif]]

more details can be found under: [[notes/Vegetation Erosion Prediction |Vegetation Erosion Prediction]]

