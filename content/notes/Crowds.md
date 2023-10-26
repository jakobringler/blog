---
title: Crowds
draft: true
tags:
  - houdini
  - cfx
  - fx
---
The following notes only cover crowds with rigged agents, which are evaluated in real time during the simulation. Simple setups with instanced geometry caches are not covered here but may just as well be a viable solution.

## Agents

Every crowd instance is an agent.

### Agent Definition

An agent definition typically consists of a rig, one or more agent layers, which can hold different geometry (or other) variations and one or more agent clips, which store different animations for the rig.

To start creating an agent definition you need the `agent` SOP.

### Agent Layers

Agent layers are used to store different geometry variations in the same base agent. You can for example attach different faces, hairstyles, clothing or other accessories to the agent definition, which can be selected or randomized with attributes later on. To attach something to you agent you can use the `agent layer` SOP.

##### Attaching Rigid Geometry (Accessories, Weapons etc.)

Anything you want to attach, has to have a name attribute. You can pin it to the rig by choosing a bone which is used to transform the geometry correctly. See example below:

![[notes/images/agent_layer_assign.png]]

If you want to add something to your base geometry, make sure to activate `Source Layer` under `Use Existing Shapes` and set it to whatever you chose to name your default layer.

##### Attaching Deforming Geometry (Clothes etc.)



### Agent Clips


### Debugging your Agent

To visualize a specific agent (correct layers & clip) you can use the `agent edit` SOP. 

To preview a crowd and randomize different aspects you can use the `crowd source` SOP in combination with `crowd assign layers`.

## Animation

### In Place Animation

### Locomotion

### Clip Properties

### Clip Transitions


## Simulation

### Triggers

### Forces

### Ragdolls

### Partial Ragdolls


## Rendering

### Material Style Sheets

---

sources / further reading:
- [Zombies for Everyone | Quick Intro to Crowds - SideFX](https://www.sidefx.com/tutorials/zombies-for-everyone-quick-intro-to-crowds/)
- [Intro to Crowds - SideFX](https://www.sidefx.com/tutorials/intro-to-crowds/)

