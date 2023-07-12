---
title: "Exporting Image Sequence"
draft: false
tags:
- touchdesigner
---

- drop down a `Movie File Out` TOP and choose image sequence
- put the following snippet as an expression in the file path 
``` Python
'myImgSeq' + me.fileSuffix.replace( str(tdu.digits( me.fileSuffix )), f"{tdu.digits( me.fileSuffix ):04}")
```
- set TouchDesigner to non-Realtime mode by toggling off the Realtime Button at the top of the screen (this avoids dropping any frames)
- go to the beginning of your timeline
- turn on `Record` on the Movie File Out TOP and hit play

---

sources / further reading:
- [TouchDesigner Forum - Derivative](https://forum.derivative.ca/t/resolved-export-movie-image-sequence-possible/5185)
- [TouchDesigner Forum - Derivative](https://forum.derivative.ca/t/number-padding-for-moviefileout-me-filesuffix/249397)

