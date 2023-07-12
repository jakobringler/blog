---
title: "Feedback Loops"
draft: false
tags:
- touchdesigner
---

Like a [[notes/SOP Solver |SOP Solver]] in Houdini!

The setup is a little different but the same logic applies. A TOP feedback loop is regularly used to blur, fade or transform pixels like you usually do with attributes in Houdini.

The most common setup is built using a feedback and a comp node like this:

![[notes/images/touchdesigner_basic_feedbackloop.gif]]

In the feedback node I pointed to the comp9 node which overlays the feedback with the current frame. Everything in between the `comp9` node and the feedback node can be seen as the inside of a SOP solver.

In this example I used a level node to multiply the brightness with 0.95 every frame and blur it a little which leads to this fading effect.

---

sources / further reading:
- [The Secret of Feedback Loops in TouchDesigner - Tutorial](https://www.youtube.com/watch?v=JkKHKsY31no)

