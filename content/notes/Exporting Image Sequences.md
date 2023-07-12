---
title: "Exporting Image Sequences"
draft: false
tags:
- touchdesigner
---

- set your timeline in and out options to the desired length
- drop down a `Movie File Out` TOP and choose image sequence
- put the following snippet as an expression in the file path 
``` Python
'myImgSeq' + me.fileSuffix.replace( str(tdu.digits( me.fileSuffix )), f"{tdu.digits( me.fileSuffix ):04}")
```
- to also export audio reference the audio CHOP in the related parameter
- set TouchDesigner to non-Realtime mode by toggling off the Realtime Button at the top of the screen (this avoids dropping any frames)
- set the loop option to `once`
- go to the beginning of your timeline
- turn on `Record` on the Movie File Out TOP and hit play

---

sources / further reading:
- [TouchDesigner Forum - Derivative](https://forum.derivative.ca/t/resolved-export-movie-image-sequence-possible/5185)
- [TouchDesigner Forum - Derivative](https://forum.derivative.ca/t/number-padding-for-moviefileout-me-filesuffix/249397)
- [Exporting – TouchDesigner Tips, Tricks and FAQs 1 - bileam tschepe (elekktronaut)](https://www.youtube.com/watch?v=_1r8KLZWZ20)

