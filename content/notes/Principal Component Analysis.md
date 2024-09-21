---
title: Principal Component Analysis
draft: true
tags:
  - machinelearning
  - math
  - houdini
---
just a method of summarizing some data. 
Imagine you have some wine bottles standing in front of you on the table
We can describe each wine by its colour, how dry or sweet it is, how strong it is, how old it is, and so on.
We can compose a whole list of different characteristics of each wine.

wine bottle image

But many of them will measure related properties and so will be redundant. If so, we should be able to summarize each wine with fewer components! This is what PCA does.
Instead of say 10 you may end up with 3 components that describe your data accurately enough
Never 100% but close enough

table image

PCA is not selecting some characteristics and discarding the others. Instead, it constructs some new characteristics called components that turn out to summarize our list of wines well. 
( > linear combinations).
In fact, PCA finds the best possible characteristics, the ones that summarize the list of wines as well as only possible. 
Those components are ordered by importance, which comes in handy later.

table 2 image

In 2d this could look something like this 
You have 2 axis, color and age and you have a scatter plot of different wines.
This is of course not how wine works but lets just pretend that it gets more colorful over time.
If those two properties are related like that you can compress them because both store the same underlying information > time passed

2d image 1

 the first pca component for that data looks like this:
It’s the direction through your data where the most variance occurs. The longest distance you could say

2d image 2

The second component would look like this
Perpendicular to the first one
Second most important variance 

2d image 3

What you can do then is project your original data samples onto the new components and calculate weights (how much of which component do you need) to reconstruct the original data point
In 2d this isn’t very useful and doesn’t compress anything, but in highdimensional space you can compress your data a lot, while still maintaining most of the valuable information

2d image 4

Not important to understand the math completely but how to use it and what its good for

How is this useful for cg?

- [[notes/Bounding Box Orientation on Arbitrary Point Clouds with PCA|Bounding Box Orientation on Arbitrary Point Clouds with PCA]]
- [[notes/Geometry Denoise with PCA|Geometry Denoise with PCA]]
- [[notes/Volume Denoise with PCA|Volume Denoise with PCA]]
- [[notes/ML Sketching|ML Sketching]]
- [[notes/ML Groom Deformer|ML Groom Deformer]]

---

sources / further reading:
- [Data Analysis 6: Principal Component Analysis (PCA) - Computerphile](https://www.youtube.com/watch?v=TJdH6rPA-TI)
- [Really Good PCA explanation - stats.stackexchange](https://stats.stackexchange.com/questions/2691/making-sense-of-principal-component-analysis-eigenvectors-eigenvalues/140579#140579)

