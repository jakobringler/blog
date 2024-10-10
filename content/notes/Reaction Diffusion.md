---
title: Reaction Diffusion
draft: false
tags:
  - houdini
---
Reaction-diffusion is a concept borrowed from biology, and in computer graphics, it helps create interesting textures and patterns, like the ones you see on animal skins or in nature.

![[notes/images/reactiondiffusion_banner.jpg]]

In Uni I worked on two "shortfilm" projects using ferrofluids that show this effect really well in reality:
- [CONVERGENCE](https://vimeo.com/393791260)
- [INFECTION](https://vimeo.com/452001942)
### How It Works:

1. **Two Chemicals**: Imagine there are two substances, often called A and B. They react with each other and spread out in space (like a spill).

2. **Reaction**: When A and B mix, they change each other. For example, A might help create more of B, while B might slow down A. This interaction is what creates interesting patterns.

3. **Diffusion**: Both substances also move around over time. They "diffuse" out from where they're concentrated to where they're less concentrated, like how a drop of food coloring spreads in water.

4. **Feedback Loop**: The combination of the reaction and the diffusion creates a feedback loop. Sometimes, this leads to stable patterns (like stripes or spots) or dynamic ones that change over time. Those stable patterns are what we are usually after when trying to create this is CG.
### In Graphics:

You can use reaction-diffusion to generate textures in 2D or 3D graphics. By tweaking the rules of how A and B interact and spread, they can create unique and complex designs that look organic.

I recommend taking a look at Karl Sims' [browser demo](https://www.karlsims.com/rdtool.html) and all the odforce threads I linked below.

---

sources / further reading:
- [Reaction-Diffusion Tutorial - Karl Sims](http://www.karlsims.com/rd.html)
- [Fun with Reaction-Diffusion in Houdini | Dan Wills | SIGGRAPH Asia 2019](https://www.youtube.com/watch?v=K_7TkoIkFhk)
- [Differential curve growth - Odforce](https://forums.odforce.net/topic/25534-differential-curve-growth/)
- [Houdini Reaction Diffusion - Odforce](https://forums.odforce.net/topic/25906-houdini-reaction-diffusion/)
- [Diffusion - Odforce](https://forums.odforce.net/topic/24406-diffusion/)

