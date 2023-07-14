---
title: "TD SOPs"
draft: false
tags:
- touchdesigner
---

## Basics

SOPs are computed on the CPU and therefore don't scale too well. Animation in SOP land is dangerous since every frame has to be recomputed live.

In active viewer mode you can right click to get different display options:

![[Pasted image 20230714142920.png]]

It generally makes sense to turn the viewer of when working with SOPs. TD has to calculate every viewer for every timestep otherwise. To do so press the viewer flag in the top left of the operator.

![[Pasted image 20230714145122.png]]

## SOPs

SOPs have the most overlap with [[notes/Houdini |Houdini]]

### Basic Shapes
- Circle
- Rectangle
	- Single Polygon
- Grid
- Box
- Sphere
- Tube
- Torus

### Merge
- combine geo

### Transform
- move stuff

### Attribute Create
- generates Normals or Tangents for existing geometry
- usually necessary before rendering GEO

### Sort 
- change point order based on rule

### Particles
- emits points from GEO
- to get random look you have to sort the input geo point order randomly
- to view particles in the viewport/render you have to apply a `pointsprite` material
- gets rather slow very quickly (CPU single threaded)

## Setups

### Basic Render Setup

To do so you need a `Geo`, a `Camera` and a `Light` COMP as well as a `Render` TOP

![[Pasted image 20230714144235.png]]

You can build simple control setups by providing a `null` for the camera to look at.

---

sources / further reading:
- [10 – SOPs – TouchDesigner Beginner Course - bileam tschepe (elekktronaut)](https://www.youtube.com/watch?v=JfBNyy47YU8)