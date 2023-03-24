---
title: "Collisions"
draft: false
tags:
- houdini
- fx
---

### Performance

To avoid regenerating collision geometry / vdbs of a moving but nondeforming object it makes sense to avoid SOP level animation and only animate on OBJ level and use the transform to move the collider in the DOP network. To do so you have to point the `OBJ Path` Parameter in the static object to the geo node containing your collider and enable `Use Object Transform`. This speeds up collider calculations massively.

![[notes/images/collision base setup.png]]

### Collision VDBs from Bad Topology

When dealing with bad or damaged / 'non watertight' topology you can use the `VDB Topology to SDF` node to approximate a surface from the bad vdb result you get from the `VDB from Polygons` node.

![[notes/images/badtopofix.png]]

---

sources / further reading:
- 

