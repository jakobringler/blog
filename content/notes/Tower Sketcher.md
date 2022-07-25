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

### Data Synthesis and PDG

![[notes/images/TowerDataset.png]]

### Results

![[notes/images/TowerSketcher.gif]]
// gif plays at 2x speed

![[notes/images/towerSketchdemo.png]]

Since all the resulting values get clamped to a plausible range everything outputs a somewhat good tower, which results in those rather incoherent pairs:

 ![[notes/images/towerSketchdemo2.png]]