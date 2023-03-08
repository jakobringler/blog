---
title: "Retiming Techniques for Pyro"
draft: false
tags:
- houdini
- fx
---

### Timescale in Simulation

The Timescale parameter let's you control the speed at which a solver simulates. However you have to make sure to scale up or down any forces that affect velocity accordingly. Using timescale to create slow motion might not always be the best solution, as it directly increases simulation time. (more frames more time etc.) Also matching a result you already have set up in slow motion is pretty tough.

### Post Sim Retime & Timeshift

When retiming volumes you can use the vel field from the simulation to advect the volume and calculate in between frames. Unfortunately this causes flickering as you jump from fully simulated sharp frames to  interpolated ones which tend to be a little more blurry. 

(e.g 10 10.5 11 11.5 etc.)

To fix this you can use a timeshift after the retime node and offset every frame by 0.25. Make sure to disable `Integer Frames`.

```C
$FF+0.25
```

This gives you only interpolated frames and reduces flickerung drastically.

(e.g. 10.125 10.625 11.125 11.625 etc.)


---

sources / further reading:
- [Volume Retime Strategies #Houdini #VDB #Pyro Stream 2020 11 18 - John Kunz](https://www.youtube.com/watch?v=m48ynuhEFKY)
- [Axiom Fundamentals - 17 - Time Scale ( Slow-Motion ) - Theory Accelerated](https://www.youtube.com/watch?v=U6NsyCPfCI0)

