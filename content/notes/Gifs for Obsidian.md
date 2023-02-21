---
title: "Gifs for Obsidian"
draft: false
tags:
- linux
---

## ffmpeg from mp4 to pngs
// bash
```bash
ffmpeg -i video.mp4 -vf scale=1280:720,fps=15 pngs/video.%04d.png
```

## gifski from pngs to good quality gif
// basterminalh
```bash
gifski -o video.gif -W 1280 -H 720 pngs/video.*.png
```


---

sources / further reading:
- [FFmpeg: high quality animated GIF?](https://stackoverflow.com/questions/42980663/ffmpeg-high-quality-animated-gif)

