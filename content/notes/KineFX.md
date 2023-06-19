---
title: "KineFX"
draft: false
tags:
- houdini
---

## Misc

### Display Rig Controls on HDA Viewer State

To expose the control rig on an HDA you have to define the "Default State" parameter in the "Node" tab of the Operator Type Properties. 

For kineFX controls this should be `kinefx__rigpose`.

You also have to promote the parameters you want to control with the HDA.

---

sources / further reading:
- [KineFX Rigging | Fur Dude | Part 9 | Add More Control Joints - SideFX](https://www.youtube.com/watch?v=3Ssa9xXnx9E)

