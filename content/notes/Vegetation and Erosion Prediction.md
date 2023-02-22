---
title: "Vegetation Erosion Prediction"
draft: true
tags:
- houdini
- machinelearning
- python
- pytorch
---

## Predicting Heightfield Data using cGANs

### Concept

Hf ist just image
![[notes/images/heightfield to pixel.png]]

Scatter Masks
![[notes/images/vegetationMaskToGeo.png]]

### Pix2Pix

what is it

### Setup

![[notes/images/Houdini Heightfield Prediction using Pix2Pix.jpg]]

### Data Synthesis and PDG

noise terrain / eroded terrain
![[notes/images/noiseterraintoerossion.png]]
Data Sample (Input & Output)
![[notes/images/erosionsample.png]]

Data Sample (Input & Output)

![[notes/images/vegetationsample.png]]

### Results

###### Erosion

![[notes/images/ErosionPrediction.gif]]
Result (Ground Truth left / Prediction right)
![[notes/images/heightfieldGroundtruthvspredicted.png]]
Error Visualized
![[notes/images/HeightfieldErrorVis.png]]
##### Vegetation

![[notes/images/VegetationDemo.gif]]
Predicted Masks (left) & Scattered Assets (right)
![[notes/images/vegetationToScattering.png]]
### Outlook

If provided with the right real world data set you could create a scatter system that matches the vegetation of certain regions in the world fairly easy.