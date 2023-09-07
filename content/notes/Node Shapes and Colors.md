---
title: "Node Shapes and Colors"
draft: false
tags:
- houdini
- python
---

### Getting All Node Shape Names

```Python
# Get All Node Shape Names

editor = hou.ui.paneTabOfType(hou.paneTabType.NetworkEditor)

print(editor.nodeShapes())

# All available Node Shape Names

# ('rect', 'bone', 'bulge', 'bulge_down', 'burst', 'camera', 
# 'chevron_down', 'chevron_up', 'cigar', 'circle', 'clipped_left', 
# 'clipped_right', 'cloud', 'diamond', 'ensign', 'gurgle', 'light', 
# 'null', 'oval', 'peanut', 'pointy', 'slash', 'squared', 'star', 
# 'tabbed_left', 'tabbed_right', 'tilted', 'trapezoid_down', 
# 'trapezoid_up', 'wave')
```

### Setting a Default Color and Shape for an HDA

Create a `OnCreated` script in the Scripts tab of the type properties of the HDA and paste the following code:

```Python
kwargs["node"].setColor( hou.Color(1,0,0) )
kwargs["node"].SetUserData( 'nodeshape', 'wave')
```

---

sources / further reading:
- [List of available Houdini node shapes - Morphingdesign](https://gist.github.com/morphingdesign/0d1af23664a8ce356fca0c7668f4dd85)
- [Houdini: Organizing and managing - Simon Houdini](https://www.youtube.com/watch?v=vwB-gY7j22s)