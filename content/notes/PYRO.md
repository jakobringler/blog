---
title: "PYRO"
draft: true
tags:
- houdini
- fx
---

## Sourcing

[[notes/Attribute Adjust Syntax Tricks |Attribute Adjust Syntax Tricks]]

### Pyro Burst Source
Notes I made for Attila Toroks talk linked below.

- generates animated points in random shapes
	- can be sourced as volumes for pyro
- size and shape controls (no need for base pop sim)
- can create attributes (temperature, burn, density, etc.)
- provides quick setups to streamline workflows
	- single input point: creates add node before burst node to enable easy transforms etc.
	- source volume: rasterizes attributes and connects all components correctly
	- 
#### Single Burst
- Controlling Source Values:
	- Scale attributes along trail
	- Noise
	- Start Frame overrides
	- seed offset
#### Multiple Bursts
- use multiple input points
- randomize attributes and frame offsets per input point in the `Burst Animation` parameter tab
- use attribute adjust button next to attributes name to create node and control attribute values
### Pyro Trail Path
### Pyro Trail Source

### Retiming

John Kunz recorded a great demo about retiming volumes. You can find my notes here: [[notes/Retiming Techniques for Pyro |Retiming Techniques for Pyro]]

## Affect Rigid Body Pieces in Pyro Simulation

To achieve a floaty look you can crank up the feedback scale under the `Collisions` tab of the pyro solver.

---

sources / further reading:
- [Sexy Explosions | Attila Torok | Houdini 18.5 HIVE](https://www.youtube.com/watch?v=Gxfq9DZTuRM)

