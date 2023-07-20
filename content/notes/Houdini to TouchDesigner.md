---
title: "Houdini to TouchDesigner"
draft: false
tags:
- touchdesigner
---

[[notes/TouchDesigner |TouchDesigner]] was born as a fork of [[notes/Houdini |Houdini]] 4.1, which shows in some methodologies and naming conventions. From a [[notes/Nuke |Nuke]] compositing standpoint there are many similarities when it comes to 2D image manipulation as well.

> [!info] **Disclaimer:**
> 
> The following are mostly the notes I took when starting to learn TD. Don't read this if you want to get started.
> I instead recommend to check out Bileam Tschepe's [Beginner Course](https://www.youtube.com/playlist?list=PLFrhecWXVn5862cxJgysq9PYSjLdfNiHz).
> This might only be useful if you are looking for some Houdini analogies!

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
- `A` activate selected viewer
- `H` center viewport
- `W` wireframe mode in SOP viewport
- `N` toggle big names in network graph
- `MiddleMouse Click` on nodes/operators gives you all the information like in Houdini
- `Mouse Click` on the outer edge of an operator resizes the viewport to fit the aspect ratio
- `P` in SOPs viewer active mode opens display options

### Designer vs Perform Mode

When working you are in "Designer" mode. This is where you see your network and the TD UI. Perform mode is a setting that hides all the UI and only Displays the output of your network in a new window.
You can activate "Perform" mode by pressing `F1` and exit it by hitting `ESC`. Closing the window with the windows/macOS buttons closes TD fully.

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

### Node UI

- Star: Viewer Active
- Arrow: Bypass
- Lock: Unlock/Lock
- Target Circle: Display
- Dot: Activate BG Display

### Comparison

##### Hou to Touch

- Subnet > Base
- Poly Frame > Attribute Create (only generates normals and tangents)
- Merge = Merge 
- Transform = Transform
- Sort = Sort
- circle, grid, etc. are mostly the same

##### Nuke to TOPs

- Grade, Color Correction, Invert > Levels
- Saturation, Hueshift etc. > HSV Adjust
- Merge > Composite
- Reformat > Resolution
- Shuffle > Channel Mix (similar to old shuffle style)
- Crop = Crop
- Blur = Blur
- Transform = Transform

### Examples

Under the `Help` tab you can find `Operator Snippets` where you can load examples for most of the nodes

## Workflow

###  Similarities to Houdini

- always use `null` nodes to mark the end/output of something (especially useful when the output is referenced somewhere)
- 'Copy Parameter' / 'Paste Reference' Workflow is the same
- You can `Lock`, `Bypass`, and `Display` nodes

### Random Facts

- in TD the axis of any image (TOPs) are mapped to `uv coordinates` ranging from 0-1 (think STMap in Nuke)

### HDAs to MyComponents



---

sources / further reading:
- [TouchDesigner Glossary - Derivative](https://docs.derivative.ca/TouchDesigner_Glossary)
- [Operator - Touchdesigner DOCs - Derivative](https://docs.derivative.ca/Operator)
- [TouchDesigner Beginner Course - bileam tschepe (elekktronaut)](https://www.youtube.com/playlist?list=PLFrhecWXVn5862cxJgysq9PYSjLdfNiHz)
- [Why To Use NULLs – TouchDesigner Tips, Tricks and FAQs 2 - bileam tschepe (elekktronaut)](https://www.youtube.com/watch?v=u6hb-31gd1Q)


