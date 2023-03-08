---
title: "AXIOM"
draft: true
tags:
- houdini
- fx
---

## Sourcing

### Emitter Naming Conventions

-   **density**
-   **temperature**
-   **fuel**
-   **vel** _(VDB vector type)_
-   **pressure** _(divergence)_
-   **color** _(VDB vector type)_
-   **sink**
-   **pump**
-   **collision** (Fog volume or SDF)
	-   **collisionTemperature**
	-   **collisionVel** _(VDB vector type)_
-   **influence**
	-   **influenceTemperature**
	-   **influenceFuel**
	-   **influenceVel** _(VDB vector type)_
	-   **influencePressure** _(divergence)_

## Collisions

## Forces

## Control Fields

## Combustion

## Caching

## Time Scale

Same things as in [[notes/Retiming Techniques for Pyro |Retiming Techniques for Pyro]] apply.

---

sources / further reading:
- [Axiom Fundamentals Workshop - Theory Accelerated](https://www.youtube.com/playlist?list=PLjdKZQKYXc1uvPhw6klTpwhVUizBqop4B)
- [Axiom Documentation](https://www.theoryaccelerated.com/documentation)