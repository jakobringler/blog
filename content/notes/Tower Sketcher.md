---
title: "Tower Sketcher"
draft: true
tags:
- houdini
- machinelearning
- python
- pytorch
---

## Training a Neural Net to Understand my Drawings
This project is part of [[notes/ML Castles |ML Castles]] and was created for my bachelor's thesis.

### Concept
The basic idea was to create an HDA and train a neural net to translate rough pixel sketches into parameter values that produce the drawn shape when entered in the HDA.

![[notes/images/towerHDAconcept.png]]

### Setup
To create the necessary training data I used PDG. It's pretty straight forward to create almost infinite random variations of an HDA output and write them, or any part of their data, to disk. I generated 10.000 image-parameter pairs to train the network. The training process was also done inside of PDG in a python script node. The network was a combination of a [[notes/Convolutional Neural Networks (CNNs) |CNN]] and a [[notes/Feed Forward Networks (FFNs) |FNN]]. More details further down.
 
![[notes/images/setuptowerhda(2).jpg |400]]

The Inference process (Predicting new results) was done in SOPs by running an Image (stored on a heightfield/2D Volume) through a python SOP and feeding the predicted parameters back in the HDA.

### Data Synthesis and PDG
As mentioned above the data generation was done in PDG.

![[notes/images/TowerDataset.png]]

### Results

![[notes/images/TowerSketcher.gif]]
// gif plays at 3x speed

![[notes/images/towerSketchdemo.png]]

Since all the resulting values get clamped to a plausible range everything outputs a somewhat good tower, which results in those rather incoherent pairs:

 ![[notes/images/towerSketchdemo2.png]]
 