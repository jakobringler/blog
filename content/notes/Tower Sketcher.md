---
title: "Tower Sketcher"
tags:
- houdini
- machinelearning
- python
- pytorch
---

## Training a Neural Net to Understand my Drawings

### Concept
![[notes/images/towerHDAconcept.png]]

### Setup 

![[notes/images/setuptowerhda(2).jpg |400]]

### Data
![[notes/images/TowerDataset.png]]

### Results

![Demo](https://youtu.be/SomxUXyUIFk)

![[notes/images/towerSketchdemo.png]]

Since all the resulting values get clamped to a plausible range everything outputs a somewhat good tower, which results in those rather incoherent pairs:

 ![[notes/images/towerSketchdemo2.png]]