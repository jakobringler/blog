---
title: "Tower Sketcher"
draft: false
tags:
- houdini
- machinelearning
- python
- pytorch
---

## Training a Neural Net to Understand Sketches
This project is part of [[notes/ML Castles |ML Castles]] and was created for my bachelor's thesis.

### Concept
The basic idea was to create an HDA and train a neural net to translate rough pixel sketches into parameter values that produce the drawn shape when entered in the HDA.

![[notes/images/towerHDAconcept.png]]

### HDA - Procedural Tower Generator
The base of the system is this very basic HDA that controls a single tower. The user can set different parameters to control the shape. The Asset outputs a 3D model and a 2D sketch derived from the model.

![[notes/images/towersample.png]]

![[notes/images/towerBaseBreakdown.png]]
// base breakdown

![[notes/images/towerroofbreakdown.png]]
// roof breakdown

![[notes/images/towerComparsion_.png]]
// roof types

### Setup
To create the necessary training data I used PDG. It's pretty straight forward to create almost infinite random variations of an HDA output and write them, or any part of their data, to disk. I generated 10.000 image-parameter pairs to train the network. The training process was also done inside of PDG in a python script node. The network was a combination of a [[notes/Convolutional Neural Networks (CNNs) |CNN]] and a [[notes/Feed Forward Networks (FFNs) |FNN]]. 
 
![[notes/images/setuptowerhda(2).jpg |400]]

The Inference process (Predicting new results) was done in SOPs by running an Image [[notes/Abusing Heightfields as Fast Image Canvas |(stored on a heightfield/2D Volume)]] through a python SOP and feeding the predicted parameters back in the HDA.

### Data Synthesis and PDG
As mentioned above the data generation was done in PDG. The setup is pretty straight forward:
- create `n` wedges for the parameters you want to randomize
- randomized values get stored in the pdg attribute stream
- add slight offsets to sketch lines (noise) to get a more general/robust model
- render out 2d sketch of resulting tower
- fetch image path and attributes in python script and begin training

Here is a sample of the dataset:

![[notes/images/TowerDataset.png]]



### Results

![[notes/images/TowerSketcher.gif]]
// gif plays at 3x speed

![[notes/images/towerSketchdemo.png]]

Since all the resulting values get clamped to a plausible range every sketch outputs a somewhat reasonable tower, which results in those rather incoherent pairs:

 ![[notes/images/towerSketchdemo2.png]]
 