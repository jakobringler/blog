---
title: "Hscript"
draft: false
tags:
- houdini
---

## Expressions

### Shifting Framerange in Sequence

// file parameter

```C
`padzero(4,$F+44)`
```

## Op Operators

more [[notes/Op Operators |here]]

## Textport Shenanigans

### Saving HIPs as text files

Kirill Kovalevskiy shared this technique in his [blog](https://kiko3d.wordpress.com/2015/03/19/converting-houdini-not-commercial-files/). Check it out for a longer detailed explanation!

##### Writing Files
// in hscript textport

```bash
opscript -G -r / > $TEMP/temp.cmd
```

##### Reading Back
// in hscript textport

```bash
cmdread $TEMP/temp.cmd
```

### Batch Rename Paths: opchange

```bash
opchange $HIP $JOB
```

```bash
opchange "/path/to/file" "/new/path/to/file"
```

---

sources / further reading:
- [opscript DOCs](https://www.sidefx.com/docs/houdini/commands/opscript.html)
- [Converting Houdini Not Commercial Files - Kiko3d](https://kiko3d.wordpress.com/2015/03/19/converting-houdini-not-commercial-files/)
- [cmdread DOCs](https://www.sidefx.com/docs/houdini/commands/cmdread.html)

