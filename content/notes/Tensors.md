---
title: "Tensors"
draft: true
tags:
- machinelearning
- math
---

### What are "Tensors"?

A tensor is basically a fancy way to talk about data that can have multiple dimensions. You can think of it like a multi-dimensional array. Tensors can be one-dimensional (like a list), two-dimensional (like a table), or even three-dimensional and beyond.

![[notes/images/tensordims.png]]
This image shows how more then 3d dimensions can be thought of for now:
- 4: a list of cubes
- 5: a table of cubes 
- 6: a cube of cubes
- and so on

### Image Tensors

Now, let’s relate this to images, which is a great example:

1. **1D Tensor**: Imagine a simple list of numbers. This could be like a single grayscale pixel intensity value, where each number represents the brightness of a pixel.

2. **2D Tensor**: Now, think about a black-and-white image. It’s made up of a grid of pixels, where each pixel has one intensity value. This grid is a 2D tensor!

3. **3D Tensor**: A color image, like a photo, has three RGB channels: Red, Green, and Blue. Each channel is a 2D grid of pixel values. So, when you stack these three grids together, you create a 3D tensor. The three layers (one for each color channel) hold all the color information for the image.

In summary, you can think of a tensor as a way to organize data in multiple dimensions, just like how a color image uses its RGB channels to create a rich visual experience. The more dimensions you add, the more complex the data can get, but the basic idea is all about organizing and representing that data clearly!

![[notes/images/imagetensor.png]]

---
sources / further reading:
- 