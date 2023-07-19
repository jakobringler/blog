---
title: "TD TOPs"
draft: false
tags:
- touchdesigner
---

## Basics

## TOPs

### Limit
- very versatile
- limit or loop values
- quantize values or positions

### Lookup
- great to color black and white gradients, noise etc.
- maps a grayscale value to a color picked from a ramp

## Setups

### UV Map

![[notes/images/Pasted image 20230719163105.png]]

1. Make a ramp (0 to 1)
2. rotate 90 degrees
3. combine to red and green channel

### Custom Circular Ramp

This is useful because it ramps up completely until it reaches the image borders. The circular ramp that can be generated using the ramp TOP doesn't go above 1.

![[notes/images/Pasted image 20230719162637.png]]

1. Make a ramp (-0.5 to 0.5)
2. rotate 90 degrees
3. combine to red and green channel using reorder node
4. use math node to combine channels and compute length

![[notes/images/Pasted image 20230719163035.png]]

### Color Gradients

- use a lookup node
- as the lookup just reads from the x axis of an image you can go with a Nx1 resolution where N controls the stepping of the colors (could be cool for an 8 bit look)

![[notes/images/Pasted image 20230719163708.png]]

### Instant Color Harmony

If you don't want to pick the colors for your ramps by hand there are some ways to built semi procedural color theory setups:

![[notes/images/Pasted image 20230719165027.png]]

In this example I start with a constant (1x1 pixel) and just rotate the hue twice (120 degrees) before putting everything in a layout TOP (make sure to toggle on `Scale Resolution`). This way I only have to change the one constant color to get the correct trichromatic output. Using this technique you can replicate most commonly used color harmony rules.

