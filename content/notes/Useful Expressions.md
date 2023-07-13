---
title: "Useful Expression"
draft: false
tags:
- touchdesigner
---

### Getting the Parent Resolution

especially useful when resolution is subject to change mid project

// get width and height

```
me.parent().width
me.parent().height
```

### Using Time

// set Frame or Second Time since TD started

```
absTime.seconds
absTime.frame
```

### Referencing Table DAT Cells

// get values

```
op('operatorname')[row,column].val
```

### Referencing Table Info

// get number of rows or columns

```
op('operatorname').numRows

op('operatorname').numCols
```
