---
title: "Velocity Fields"
draft: false
tags:
- houdini
---

## Advanced Velocity Fields

### Calculate Potential Flow with VDBs

![[notes/images/potential_flow_setup.png]]
You can use the `VDB Potential Flow` node to predict how air would move around an object. You can set the direction the 'wind' is coming from with the `Background Velocity` parameter on the node. To make the velocities converge behind the object you can use two cross product operations to cross the velocity field with the gradient of the sdf twice! 

//content of: doublecross_with_gradient - Volume VOP
![[notes/images/potential_flow_vop.png]]
Check out Mats video on the topic: [Houdini Tutorial: Advanced Velocity Fields / Part One - Mats](https://www.youtube.com/watch?v=K0cNvpXujmk)

### Magnetic Fields

coming soon

---

sources / further reading:
- [Houdini Tutorial: Advanced Velocity Fields / Part One - Mats](https://www.youtube.com/watch?v=K0cNvpXujmk)

