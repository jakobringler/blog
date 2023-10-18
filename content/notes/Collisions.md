---
title: Collisions
draft: false
tags:
  - houdini
  - fx
---
Most of the following are my personal notes on Jeff Wagners awesome masterclass on [Collisions](https://www.sidefx.com/community/collision-geometry-in-dop-simulations/). It's a bit on the longer side of presentations (6ish hours in 3 parts), but I highly recommend watching it! It also comes with demo files and a PDF which I will quote multiple times below.

## How Colliders are built in DOPs

DOPs is set up in a way that most HDAs are built from an empty object. Then there is data added to it. "Data" can mean loads of things:
- geometry
- physical simulation parameters
- display geometry
- render parameters to define what is visualized in the viewport

### Ground Plane

![[notes/images/groundplane_deconstruct.png]]

1. (yellow) creates volume collision data and sets render parms to make it invisible in the viewport
2. (red) applies motion/position data (also from OBJ level)
3. (green) applies physical parameters such as `bounce` and `friciton`
4. (blue) can be used to turn object active
5. (purple) fetches display geometry and sets render parms 
6. (pink) labels collider
7. (peach) stores info where the DOP object comes from
8. (orange) bullet collision setup
9. (dark green) display geometry SOP network
10. (grey blue) collision geometry SOP network

This is just a sparse overview to go back to. To really understand it watch part 1 of Jeff Wagner's presentation (00:12:00 - 00:25:00).
### Static Object

The `static object` DOP is a little bit more involved then the ground plane, but it's based on the same concepts .. just with more boilerplate nodes and switch logic around it.

![[notes/images/staticobject_deconstruct.png]]

I matched the node colors to show the similarities.

### Deforming Object

This just exists as a shelf tool which sets up the `static object` DOP correctly to account for transformations and deformations.

## Different Solvers Expect Different Colliders

### POP Solver

Supported Collision data:
1. SDF Volume geometry (default)
	- very fast, real 3D
2. Height Fields (same as SDF Volume data) 
	- very fast but no holes
	- only `unchanged`, `stop`, `die` response / hit behaviour work
3. Polygonal geometry
	- slow ( has to calculate primitive, primuvs for each particle )
	- can be used for complex systems that depend on per hit data like `hitnormal` etc.
	- `slide`, `stick` response / hit behaviour only works with polygons

Particle simulations tend to be either of the following two types:
1. A LOT of dumb particles
2. A FEW smart particles

> // from the SideFX pdf:
> 
> Dumb particles have no real rules other than forces motivating them through space. Using the default SDF Volume type colliders is the preferred method. The test to see if a particle is in or out of an SDF is a continuous linear lookup. The solver can predict when a particle is about to go from +ve outside space to =ve inside space of the SDF Volume. Remember the SDF is built from surface geometry. It encodes the surface upon SDF creation so all the hard work has been done. 
> Am I in or out? SDF look up is almost instantaneous. 
> 
> If you have smart particles with lots of rules especially around collisions, you can choose to use polygons. When you collide against polygons, you can inherit the bi-linearly interpolated attribute values from the polygon faces. This means that you need to record the polygon face number and the actual local u and v hit co-ordinates on that polygon face. 

To make particles collide with polygons you can use  the `POP Collision Detect DOP` in the pop source stream of the simulation. Just give it a SOP path and it should work.

![[notes/images/Pasted image 20231018130122.png]]

##### Particle Orientations and Collisions

> // from the SideFX pdf:
> 
> When particles collide against a surface, the velocity vector of the particle is mirrored along the hit normal or the tangent of the face. This works for both SDF Volume colliders and polygonal surface colliders. 
> 
> By default, any instanced or copied geometry to these points will use the `v` velocity attribute to orient the geometry, if there is no `vector4 orient` quaternion attribute on the geometry

### Grain and Wire Solver

Grains uses the pop solver and the wire solver behaves like POPs so the same rules apply.

Supported Collision data: 
1. SDF Volume geometry (default) 
2. Height Fields (same as SDF Volume data) 
3. Polygonal geometry

### FLIP Solver

Unlike the POP Solver which can take different but separate collider types the [[notes/FLIP |FLIP]] solver needs polygonal geometry **AND** a surface vdb!

Supported Collision data:
1. SDF Volume data for surface collisions 
2. Height Fields (same as SDF Volume data) 
3. Volume Velocity Fields 
4. (Polygon surfaces through POP Collision Detect but at your own peril)

On the FLIP solver under `Volume Motion` > `Collision` you can specify where to read the collision velocities from (Point or Volume).

> // from the SideFX pdf:
> 
> The main issue is resolution. For test FLIP sims and detailed colliders, you will not have enough collision detail. You have to enable.
> 
> Thin colliders like cups and vessels containing fluid can benefit from: • Thick boundary walls.
> • if moving fast, lots of substeps and blending between frames. (Default shelf setup). 
> • Extra velocity on the collider adding in some surface normals to the velocity.

To avoid precision errors make sure to simulate in a moderate scene scale ( not too small or too big ). I wrote a little more about it [[notes/General Workflow Tips#Simulation Scale |here]].

### PYRO Solver

Supported Collision data:
1. SDF Volume data for surface collisions 
2. Height Fields (same as SDF Volume data) 
3. Volume Velocity Fields

> // from the SideFX pdf:
> 
> You can use particles as collider sources using the `Gas Particle To Field` DOP to take your points and convert them in to SDF collision fields for use by the Pyro Solver. 
> 
> You need to name the field “collision”. 
> 
> If you go this route, you also have to use a second `Gas Particle To Field` DOP to construct a companion “collisionvel” vector field to hold the velocity of your collider. \[...]
> 
> Same goes for FLIP colliders. How these merge in to other existing collision and collisionvel fields is determined by how you mix these in with the `Gas Linear Combination` DOP

### CLOTH Solver
### SOLID (FEM) Solver 

### RBD Solver

WIP

### VELLUM Solver

## General Collider Workflows
### Concave Colliders with Convex Hulls ( Bullet )

Default bullet colliders are convex:

![[notes/images/Pasted image 20231018110552.png]]

To work around this without sacrificing performance too much there are two similar ways. Both split up the geometry into smaller pieces which will be "convex hulled" separately. Make sure to enable `Create Convex Hull per Set of Connected Primitives`!

1. Voronoi Fracture

![[notes/images/Pasted image 20231018110815.png]]

2. Convex Decomposition

![[notes/images/Pasted image 20231018111022.png]]

### Collision VDBs from Bad Topology

When dealing with bad or damaged / 'non watertight' topology you can use the `VDB Topology to SDF` node to approximate a surface from the bad vdb result you get from the `VDB from Polygons` node.

![[notes/images/badtopofix.png]]

### Performance

To avoid regenerating collision geometry / vdbs of a moving but nondeforming object it makes sense to avoid SOP level animation and only animate on OBJ level and use the transform to move the collider in the DOP network. To do so you have to point the `OBJ Path` Parameter in the static object to the geo node containing your collider and enable `Use Object Transform`. This speeds up collider calculations massively. However you will lose per point collider velocities, because the geometry isn't moving in SOPs. 

![[notes/images/collision base setup.png]]

### Represent Geo with Spheres for Fast Bullet Collisions

If you need fast but pretty accurate collisions in RBD sims you can generate loads of little spheres inside the geometry and use those as a compound to calculate collisions.

![[notes/images/sphere_bullet_collider.png]]

Make sure to use `bake ODE` and set the `Geometry Representation` on the RBD Object to `Compound`. 

You can then apply the transformation back to the original geometry by using the extract transform for example.

---

sources / further reading:
- [Collision Geometry in DOP Simulations - SideFX](https://www.sidefx.com/community/collision-geometry-in-dop-simulations/) this has download links for demo files and a pdf summary
	- [Collisions - Pt 1 | Jeff Wagner | Houdini Illume Webinar](https://vimeo.com/252645795)
	- [Collisions - Pt 2 (Colliders & Bullet Simulations) | Jeff Wagner | Houdini Illume Webinar](https://vimeo.com/254343083)
	- [Collisions - Pt 3 | Jeff Wagner | Houdini Illume Webinar](https://vimeo.com/255979341)