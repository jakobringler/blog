---
title: "Matrix Operations"
tags:
- houdini
- math
- vex
---

### coming soon / WIP

### Extracting Transformation Matrix with VEX

##### Creating a Tranformation Matrix



$$
\definecolor{x}{RGB}{255,0,0}
\definecolor{y}{RGB}{0,255,0}
\definecolor{z}{RGB}{0,0,255}
\definecolor{p}{RGB}{255,255,0}

\begin{array}{rcl}
	\color{x} x-Axis \\
	\color{y} y-Axis \\
	\color{z} z-Axis \\
	\color{p} Position \\
\end{array}
\equiv
\begin{array}{rcl}
	\color{x} 1&\color{x}0&\color{x}0&0 \\
	\color{y}0&\color{y}1&\color{y}0&0 \\
	\color{z}0&\color{z}0&\color{z}1&0 \\
	\color{p}0&\color{p}0&\color{p}0&1 \\
\end{array}
$$

We don't really need the fourth column but 3x4 matrices dont "exist". 

```C
matrix transform = set(X, Y, Z, P); // create matrix
```

However this will give us the following matrix with the ones in the fourth column

$$
\definecolor{x}{RGB}{255,0,0}
\definecolor{y}{RGB}{0,255,0}
\definecolor{z}{RGB}{0,0,255}
\definecolor{p}{RGB}{255,255,0}

\Bigg\{\,\begin{array}{rcl}
	\color{x} X.x&\color{x}X.y&\color{x}X.z&1 \\
	\color{y} Y.x&\color{y}Y.y&\color{y}Y.z&1 \\
	\color{z} Z.x&\color{z}Z.y&\color{z}Z.z&1 \\
	\color{p} P.x&\color{p}P.y&\color{p}P.z&1 \\
\end{array}\,\Bigg\}
$$

To fix this we can use the setcomp() function.

```C
setcomp(transform, 0, 0, 3); // set row 1 col 4 to 0
setcomp(transform, 0, 1, 3); // set row 2 col 4 to 0
setcomp(transform, 0, 2, 3); // set row 3 col 4 to 0
```



related to:

sources / further reading:
- [Houdini Tutorial | Extracting transformation matrix with VEX - Paweł Rutkowski](https://vimeo.com/284712920)

