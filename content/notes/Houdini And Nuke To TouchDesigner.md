---
title: "Houdini And Nuke To TouchDesigner"
draft: false
tags:
- touchdesigner
---

## Interface

- `P` toggles the parameter window on top of the Network Graph
- `D` toggles displays the currently selected node in the background

## Nodes

### Contexts

**COMP** (Object Components, Panel Components) - and other miscellaneous components. Components contain other operators:

**TOPs** (Texture Operators) - all 2D image operation:
- 2D image manipulation like in COPs or normal Nuke

**CHOPs** (Channel Operators) - motion, audio, animation, control signals:
- very much like Houdini CHOPs
- manipulating values and driving parameters with them

**SOPs** (Surface Operators) - 3D points, polygons and other 3D "primitives":
- equal to SOPs in Houdini
- working with geometry

**MAT** (Material Operators) - materials and shaders:
- like MAT and SHOP nets in Houdini

**DAT** (Data Operators) - ASCII text as plain text, scripts, XML, or organized in tables of cells:
- doesn't really exist in Houdini as data is usually stored in attributes

**Custom** (Custom Operators):
- you can create custom Operators which can belong to any type with C++

### Equivalents

##### Nuke to TOPs

- Grade, Color Correction > Levels
- Saturation, Hueshift etc. > HSV Adjust
- Merge > Composite
- Reformat > Resolution
- Crop = Crop
- Blur = Blur

---

sources / further reading:
- [Operator - Touchdesigner DOCs - Derivative](https://docs.derivative.ca/Operator)

