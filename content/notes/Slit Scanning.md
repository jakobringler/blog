---
title: Slit Scanning
draft: false
tags:
  - houdini
  - cops
---
## History

Slit scanning is a cool photographic technique that captures motion over time by using a narrow slit to take images of a scene. Instead of snapping a traditional photo, it exposes only a thin strip of the image at a time. When moving that slit you expose different parts of the frame at different times. This way, it records movement in a way that creates a unique and often surreal effect.

The technique has roots in early photography, but it gained a lot of attention in the 20th century, especially with artists and filmmakers who wanted to explore time and space in their work. It’s often associated with experimental films and art installations, where the visuals can twist and stretch in unexpected ways. Think of it as capturing a moment while also showing how it unfolds over time—kind of like a time-lapse but in a single frame. 2001: A Space Odyssey is most well known to have used the effect.

It’s fascinating how something so simple can produce such mind-bending results!

## Slit Scanning on Videos

Images are cool, but videos are much cooler! Houdini being Houdini it's easy enough to set something like this up for animations. And you don't even have to use a slit to do the "scanning". you can use arbitrary values on each pixel to timeshift to the past and future which let's you create some trippy videos like this:

![[notes/images/violin-side-01.gif]]

> [!quote] Source:
> 
> The amazing RBD sim and render is from [Gurmukh Sandhu](https://ca.linkedin.com/in/gurmukh-sandhu-972ba3257), who was so kind to let me use it for this experiment

> [!idea] Inspiration:
> 
> Stephan Helbing presented a similar setup for XK Studio in an amazing [hive talk](https://youtu.be/DSgxi06vZI0?si=ObVzLCde-_7orQ7o&t=2807). My setup is loosely inspired by it. The main addition is the "straight to COPs" part, which makes it much faster to iterate

You can see the impact and destruction in some parts of the image before the ball hit the violin. 3D video? somewhat!

The setup is pretty straight forward:
- create a bunch of points that will hold your color values (should match your resolution)
- give those points uvs
- create a timeoffset attribute on each point (could be a slit or noise or anything in between)
- use the timeoffset attribute to move the points to different udims to read from different frames at the same time (your image sequence has to start on frame 1001 for this to work)
- color the points using `attrib from map`
- rasterize it all into a 2d volume and bring it into COPs for post processing or export

![[notes/images/slitscan_clickthrough.gif]]

// point wrangle "norm_xy"

```C
float norm_x = chramp("remap_x", @P.x);
float norm_y = chramp("remap_y", @P.y);

f@timeoffset = max(norm_x, norm_y);
```

// point wrangle "udim"

```C
int firstframe = chi("first_frame");
int lastframe = chi("last_frame");
int framerange = chi("frame_range");
int currframe = (int)@Frame;

firstframe += currframe - 1;
lastframe += currframe - 1;

firstframe = select(firstframe<framerange, firstframe, framerange);
lastframe = select(lastframe<framerange, lastframe, framerange);

float readframe = (int)fit01(@timeoffset, firstframe, lastframe);

@uv.x += (readframe-1) % 10;
@uv.y += floor((readframe-1)/10);
```

// point wrangle "raterize"

```C
int pt = nearpoint(1, v@P);
v@C = point(1, "Cd", pt);
```

##### Download: [File](https://github.com/jakobringler/blog/tree/hugo/content/notes/sharedfiles/video_slitscanning.hip)

---

sources / further reading:
- [Slit-scan Photography - Wikipedia](https://en.wikipedia.org/wiki/Slit-scan_photography)
- [XK studio: Morphing Reality | XK studio | OFFF HIVE 2024 - Stephan Helbing](https://youtu.be/DSgxi06vZI0?si=ObVzLCde-_7orQ7o&t=2807)

