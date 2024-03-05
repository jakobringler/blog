---
title: Obsidian Syntax
draft: true
tags:
  - obsidian
---
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6

### Code:

```C
// Code
```

[Link](website url)

**Bold**
_Italic_
**Bold text and _nested italic_ text**
***Bold and italic text***
~~Strike Through~~
==Highlight==
`Inline Code`

### Lists:

- a
- b

1. one
2. two

- [x] This is a completed task.
- [ ] This is an incomplete task.

### External Images:

![Houdini Logo |50x50](https://upload.wikimedia.org/wikipedia/commons/1/15/Houdini3D_icon.png)

> Quote

#### Lines:

***
****
* * *
---
----
- - -
___
____
_ _ _

### Comments

This is an %%inline%% comment.

%%
This is a block comment.

Block comments can span multiple lines.
%%

### Footnotes:

This is a simple footnote[^1].

[^1]: This is the referenced text.
[^2]: Add 2 spaces at the start of each new line.
  This lets you write footnotes that span multiple lines.
[^note]: Named footnotes still appear as numbers, but can make it easier to identify and link references.


### Math:

$$
\bigg[\begin{array}{rcl}
	1&0&0&0 \\
	0&1&0&0 \\
	0&0&1&0 \\
	0&0&0&1 \\
\end{array}\bigg]
$$

$$
\begin{vmatrix}a & b\\
c & d
\end{vmatrix}=ad-bc
$$

### Tables:

| Table | Col2  |
| ----- | ----- |
| Row1  | Value |
| Row2  | Value |

---

sources further reading:
- [Obsidian DOCs](https://help.obsidian.md/Editing+and+formatting/Basic+formatting+syntax)