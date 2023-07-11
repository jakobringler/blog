---
title: "SOP Solver"
draft: true
tags:
- houdini
---

## Basics
Feeds back the result as input in the next step.

## Common Solver Types
### Accumulation
### Propagation / Growth

## Common Features
### Fading
When accumulating attribute values you often want to let them fade away over time e.g. when creating wetmaps. To do so you can subtract a small decimal value or multiply that value by 0.x each frame from the accumulated attribute. You might want to clamp the value to 0 on the lower end if you use subtraction

### Dealing with Changing Point Counts on Animated Geometry

## Chainsolvers

---

sources / further reading:
- [[VEX for Algorithmic Design] E20 _ Solver Basics - Junichiro Horikawa](https://www.youtube.com/watch?v=8CrVp5CyR5s)

