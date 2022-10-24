---
title: "Houdini Crash Issue when using Virtual Displays with Parsec"
draft: false
tags:
- houdini
---

To prevent the crash add the following line to your `houdini.env`.

```bash
HOUDINI_USE_HFS_OCL=0
```

---

sources / further reading:
- [Reddit Post with same Issue](https://www.reddit.com/r/Houdini/comments/nuy6st/houdini_and_parsec_remote_pc_houdini_crashes_with/https://www.reddit.com/r/Houdini/comments/nuy6st/houdini_and_parsec_remote_pc_houdini_crashes_with/)
- [Houdini Environment Variables](https://www.sidefx.com/docs/houdini/ref/env.html)

