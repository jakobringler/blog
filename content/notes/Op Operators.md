---
title: "Op Operators"
tags:
- houdini
---

## Summary

"[Op Operators I will Never Memorize](https://www.artstation.com/blogs/mohamad_salame1/DlQG/op-operators-i-will-never-memorize)" is a fantastic blog post by Mohamad Salame that summarizes all the obscurities and pitfalls you will face when having to deal with the `op` syntax.

## Syntax

- Selecting the current node: `"."`
- Relative path: `../nodename`
- Absolute path: `/obj/nodename`

## Most Useful Ones

### HDA Files: opdef

Gives access to files stored in an HDA

```hscript
`opdef:.?filename.ext`
```

or a .hda library

```hscript
`oplib:.?filename/ext`
```

### Connected Inputs: opninputs

> [!quote] **DOCs**
> 
> Returns the number of the highest connected input. This is _not_ the number of connected inputs. If a node has four inputs and the fourth input is connected, `opninputs` will return `4`. If the first and third inputs are connected, `opninputs` will return `3`.

```hscript
`opninputs(".")`
```

### Node Name: opname

Returns the name of the specified node

```hscript
`opname(".")`
```

### Texture From COPs: op

Can be used to pipe the output of a COPs network live into a texture input

```hscript
op:`opfullpath("../cop2net1/OUT_COPS")`[$F]
```

### Use Input Name for Exports: opinput

Flexible ROP outputs

```hscript
$HIP/geo/`opinput(".", 0)`.bgeo.sc
```

![[notes/images/Pasted image 20230227162722.png]]

### Batch Rename Paths: opchange

```hscript
opchange $HIP $JOB
```

```hscript
opchange /path/to/file /new/path/to/file
```