---
title: "Houdini And Nuke To TouchDesigner"
draft: false
tags:
- touchdesigner
---

TouchDesigner was born as a fork of [[notes/Houdini |Houdini]] 4.1, which shows in some methodologies and naming conventions. From a [[notes/Nuke |Nuke]] compositing standpoint there are many similarities when it comes to 2D image manipulation as well.

## Interface

### Hotkeys

- `Tab` to create nodes
- `P` toggles the parameter window on top of the network graph
- `C` toggles color options for nodes
- `O` toggles a small map of all the nodes
- `D` toggles displays the currently selected node in the background
- `U` exits node/group and goes one layer up
- `I` enters/opens selected node/group (one layer down)
- `MiddleMouse Click` on a node output lets you connect it to a new node directly similar to clicking the output of a Houdini node before searching and adding something
- `Right Click` on connections lets you insert or add operators. The latter only connects the input and not the output of the new node

### Designer vs Perform Mode

When working you are in "Designer" mode. This is where you see your network and the TD UI. Perform mode is a setting that hides all the UI and only Displays the output of your network in a new window.
You can activate "Perform" mode by pressing `F1` and exit it by hitting `ESC`. Closing the window with the windows/macos buttons closes TD fully.

For fullscreen output you have to use a `Window Comp` which has a setting `Open` that has to be set to 1 and a drop down menu that allows you to select `borderless`.

## Nodes

### Contexts > Types / Families

**COMPs** (Object Components, Panel Components) - and other miscellaneous components. Components contain other operators:
- Panels to build UIs with buttons and what not
- Containers
- Dynamics solvers
- Import Export Nodes (FBX, USD, .. )

**TOPs** (Texture Operators) - all 2D image operation:
- 2D image manipulation like in COPs or normal Nuke

**CHOPs** (Channel Operators) - motion, audio, animation, control signals:
- very much like Houdini CHOPs
- manipulating values and driving parameters with them
- anything that's got to do with audio
- input for different sensor types or controllers (e.g. midi)

**SOPs** (Surface Operators) - 3D points, polygons and other 3D "primitives":
- equal to SOPs in Houdini
- working with geometry

**MATs** (Material Operators) - materials and shaders:
- like MAT and SHOP nets in Houdini

**DATs** (Data Operators) - ASCII text as plain text, scripts, XML, or organized in tables of cells:
- doesn't really exist in Houdini as data is usually stored in attributes

**Custom** (Custom Operators):
- you can create custom Operators which can belong to any type with C++

You can't connect operators of different types together but there are many ways to convert between types or reference outputs into parameters just like in Houdini.

### Equivalents

### Hou to Touch

- Subnet > Base

##### Nuke to TOPs

- Grade, Color Correction > Levels
- Saturation, Hueshift etc. > HSV Adjust
- Merge > Composite
- Reformat > Resolution
- Shuffle > Channel Mix (similar to old shuffle style)
- Crop = Crop
- Blur = Blur

## Workflow

###  Similarities to Houdini

- always use `null` nodes to mark the end/output of something (especially useful when the output is referenced somewhere)

### HDAs to MyComponents



---

sources / further reading:
- [TouchDesigner Glossary - Derivative](https://docs.derivative.ca/TouchDesigner_Glossary)
- [Operator - Touchdesigner DOCs - Derivative](https://docs.derivative.ca/Operator)
- [TouchDesigner Beginner Course - bileam tschepe (elekktronaut)](https://www.youtube.com/playlist?list=PLFrhecWXVn5862cxJgysq9PYSjLdfNiHz)
- [Why To Use NULLs – TouchDesigner Tips, Tricks and FAQs 2 - bileam tschepe (elekktronaut)](https://www.youtube.com/watch?v=u6hb-31gd1Q)

