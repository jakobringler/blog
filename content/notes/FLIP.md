---
title: "FLIP"
tags:
- houdini
- FX
---

## General

### Particle Velocity vs Volume Velocity
It's hard to have exact art directible control over the FLIP particles when working with volume velocities. It makes sense to modify the particle velocity directly in cases where detailed particle control is required.

### Volume Loss

To avoid losing volume in the simulation activate `Apply Particle Separation` under the `Particle Motion > Separation` tab on the flip solver.

**Important Parameters**
- Grid Scale
- Particle Radius Scale

> [!Quote] Quote from SideFX Docs
> 
>If the **Particle Radius Scale** / **Grid Scale** >= sqrt(3) / 2, then particles will never be under-resolved.

## Small Scale

### Droplets, Sheets & Tendrils
Key Parameters:
- Velocity Transfer (APIC Swirly)
- Surface Tension
- Substeps
- Timescale

When increasing the surface tension by itself the fluid will collapse in on itself. To prevent this substeps have to be increased. If possible this can also be countered by reducing the timescale (e.g. instead of increasing substeps from 4 to 8 the timescale could be set to 0.5).

### Surface Adhesion
You could build a particle based setup in which you use the normal of the collider to build some kind of force to pull the fluid towards the surface. Unfortunately this causes issues with velocities when particles hit the collision from above and the adhesion force constantly fights gravity causing the simulation to slow down.

Luckily there is a better way with less shortcomings:

The `Gas Stick On Collision` node is perfect for the job and can be piped in the `Volume Velocity` input of the flip solver directly. 

It has parameters to control the scale and distance of the effect as well as separate controls for the scale along the normal and tangent. The bias parameter remaps the linear falloff along the distance. For realistic results use a higher normal scale and a lower tangent scale as well as a lower bias. This avoids slow moving fluid while insuring the adhesion effect. 

You can combine this node with the `Use Friction and Bounce` option in the flip solver to get some bouncy motion back when the fluid is fast enough.

---

sources / further reading:
- [FLIP Solver DOCs](https://www.sidefx.com/docs/houdini/nodes/dop/flipsolver.html)
- [HOUDINI FLIP | LIQUID MORPH TUTORIAL -- Part 2 (+ projects) -  Sadjad Rabiee](https://www.youtube.com/watch?v=5sD9uTsewVI)
- [Efficient techniques for realistic small scale Tendrils, Droplets and Sheets in Houdini - Jacktone Okore](https://www.youtube.com/watch?v=rxxR3hFYqLg)
- [Volume Loss in FLIP SIM? Grid Scale and Particle Radius Scale - Janks](https://www.youtube.com/watch?v=JTyPcg5x6b8)
- [Houdini | Adhesive FLIP Simulations | Masterclass - CG Forge](https://www.youtube.com/watch?v=gOgtdIk0QiI)
