---
title: "Today I Learned"
draft: false
tags:
- houdini
---

The following is a compilation of all sorts of handy houdini tips & tricks. Most of them were shared in the #today-i-learned channel on the [Think Procedural Discord Server](https://thinkprocedural.com/).
Do yourself a favor and check it out an scroll to the top of the thread to find some hidden secrets!

## Quality of Life

### Network 

- Pressing `8` in the network view toggles wire auto connection on and off (very handy if you move nodes around and don't want to mess up your setups)
- Drag a wire in the middle of an unconnected node to instantly wire up in- **and** output
- Drag & drop a camera node in the viewport to look through that camera
- You can save geometry of any node through the right click menu
- Use shortform search to find nodes:
	- `aw` -> Attribute Wrangle
	- `om` -> Object Merge
	- `gd` -> Group Delete
	- `pv` -> Point Velocity
	- `hf` -> Height Field
- if you capitalize node names they will be sorted on top in the tree view (useful when object merging stuff)
### Viewport

- `Ctrl` or `Shift` + `Arrow Keys` let's you rotate / move views
- `M` enables first person movement with `WASD`, `Shift` to sprint, `Ctrl` to slowdown and `Q` / `E` to lift / descend
- `Ctrl + Alt + S` enters 'Zen' mode and removes most of the UI clutter

### File I/O

- you can import anything straight into geo nodes by using `File`>`Import`>`Geometry`. This is very useful to import alembics without having to create a geo node and an alembic node inside it first.

---

sources / further reading:
- [Think Procedural Discord Server](https://thinkprocedural.com/)

