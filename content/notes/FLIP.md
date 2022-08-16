---
title: "FLIP"
tags:
- houdini
- FX
---

## General

### Particle Velocity vs Volume Velocity
It's hard to have exact art directible control over the FLIP particles when working with volume velocities. It makes sense to modify the particle velocity directly in cases where detailed particle by particle control is required.

### Volume Loss
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

When increasing the surface tension by itself the fluid will collapse in on itself. To prevent this substeps have to be increased. If possible this can also be countered by reducing the timescale (e.g. instead of increasing substeps from 4 to 8 the timescale could be set to 0.5). This way the simulation doesn't get more computationally expensive.


---

sources / further reading:
- [FLIP Solver DOCs](https://www.sidefx.com/docs/houdini/nodes/dop/flipsolver.html)
- [HOUDINI FLIP | LIQUID MORPH TUTORIAL -- Part 2 (+ projects) -  Sadjad Rabiee](https://www.youtube.com/watch?v=5sD9uTsewVI)
- [Efficient techniques for realistic small scale Tendrils, Droplets and Sheets in Houdini - Jacktone Okore](https://www.youtube.com/watch?v=rxxR3hFYqLg)
- [Volume Loss in FLIP SIM? Grid Scale and Particle Radius Scale - Janks](https://www.youtube.com/watch?v=JTyPcg5x6b8)


