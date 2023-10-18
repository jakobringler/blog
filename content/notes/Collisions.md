---
title: "Collisions"
draft: false
tags:
- houdini
- fx
---

### Concave Colliders with Convex Hulls ( Bullet )

Default bullet colliders are convex:

![[notes/images/Pasted image 20231018110552.png]]

To work around this without sacrificing performance too much there are two similar ways. Both split up the geometry into smaller pieces which will be "convex hulled" separately. Make sure to enable `Create Convex Hull per Set of Connected Primitives`!

1. Voronoi Fracture

![[notes/images/Pasted image 20231018110815.png]]

2. Convex Decomposition

![[notes/images/Pasted image 20231018111022.png]]
### Performance

To avoid regenerating collision geometry / vdbs of a moving but nondeforming object it makes sense to avoid SOP level animation and only animate on OBJ level and use the transform to move the collider in the DOP network. To do so you have to point the `OBJ Path` Parameter in the static object to the geo node containing your collider and enable `Use Object Transform`. This speeds up collider calculations massively.

![[notes/images/collision base setup.png]]

### Collision VDBs from Bad Topology

When dealing with bad or damaged / 'non watertight' topology you can use the `VDB Topology to SDF` node to approximate a surface from the bad vdb result you get from the `VDB from Polygons` node.

![[notes/images/badtopofix.png]]

---

sources / further reading:
- [Collisions - Pt 1 | Jeff Wagner | Houdini Illume Webinar](https://vimeo.com/252645795)
- [Collisions - Pt 2 (Colliders & Bullet Simulations) | Jeff Wagner | Houdini Illume Webinar](https://vimeo.com/254343083)
- [Collisions - Pt 3 | Jeff Wagner | Houdini Illume Webinar](https://vimeo.com/255979341)