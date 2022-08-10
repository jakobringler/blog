---
title: "Op Operators"
tags:
- houdini
---

### Summary

"[Op Operators I will Never Memorize](https://www.artstation.com/blogs/mohamad_salame1/DlQG/op-operators-i-will-never-memorize)" is a fantastic blog post by Mohamad Salame that summarizes all the obscurities and pitfalls you will face when having to deal with the `op` syntax.

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
mdread $TEMP/temp.cmd
```

---

sources / further reading:
- [opscript DOCs](https://www.sidefx.com/docs/houdini/commands/opscript.html)
- [Converting Houdini Not Commercial Files - Kiko3d](https://kiko3d.wordpress.com/2015/03/19/converting-houdini-not-commercial-files/)
- [cmdread DOCs](https://www.sidefx.com/docs/houdini/commands/cmdread.html)