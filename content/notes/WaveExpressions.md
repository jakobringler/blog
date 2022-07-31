---
title: "Wave Expressions"
tags:
- houdini
- vex
- math
---

## Useful Periodic Functions

I recently stumbled across [this](https://www.cameroncarson.com/nuke-wave-expressions) phenomenal summary of wave expressions by Cameron Carson. Check it out! He wrote everything for Nuke so I decided to build a similar little library for VEX and adjust the necessary syntax / functions.

---

Every wrangle runs on a 10 unit long resampled line along the includes this base code to setup the parameters:

```C
float offset = chf("offset");
float wavelength = chf("waveLength");
float max = chf("max");
float min = chf("min");
```

### Random

![[notes/images/vexwaves.1.jpg]]

```C
@P.y = random((@P.z+offset)/wavelength) * (max-min) + min;
```

### Noise

![[notes/images/vexwaves.2.jpg]]

```C
@P.y = noise((@P.z+offset)/wavelength) * (max-min) + min;
```

### Sin

![[notes/images/vexwaves.3.jpg]]

```C
@P.y = (sin((@P.z+offset)/wavelength)+1)/2 * (max-min) + min;
```

### Triangle

![[notes/images/vexwaves.4.jpg]]

```C
@P.y = (asin(sin(2*PI*(@P.z+offset)/wavelength))/PI+0.5) * (max-min) + min;
```

### Square

![[notes/images/vexwaves.5.jpg]]

```C
@P.y = rint((sin(2*PI*(@P.z+offset)/wavelength)+1)/2) * (max-min) + min;
```

### Sawtooth

![[notes/images/vexwaves.6.jpg]]

```C
@P.y = ((@P.z+offset) % wavelength)/wavelength * (max-min) + min;
```

### Sawtooth Parabolic

![[notes/images/vexwaves.7.jpg]]

```C
@P.y = sin((PI*(@P.z+offset)/(2*wavelength)) % (PI/2)) * (max-min) + min;
```

### Sawtooth Parabolic Reversed

![[notes/images/vexwaves.8.jpg]]

```C
@P.y = cos((PI*(@P.z+offset)/(2*wavelength)) % (PI/2)) * (max-min) + min;
```

### Sawtooth Exponential

![[notes/images/vexwaves.9.jpg]]

```C
@P.y = (exp(2*PI*((@P.z+offset) % wavelength)/wavelength)-1)/exp(2*PI) * (max-min) + min;
```

### Sawtooth Exponential Reversed

![[notes/images/vexwaves.9.5.jpg]]

```C
@P.y = (exp(2*PI*(1-(((@P.z+offset) % wavelength)/wavelength)))-1)/exp(2*PI) * (max-min) + min;
```

### Bounce

![[notes/images/vexwaves.10.jpg]]

```C
@P.y = abs(sin(PI*(@P.z + offset)/wavelength))* (max-min) + min;
```

### Blip

![[notes/images/vexwaves.11.jpg]]

```C
float bliplength = chf("bliplength");
float value = ((@P.z+(offset+wavelength)) % (wavelength+bliplength)/(wavelength)) * (wavelength/bliplength) - (wavelength/bliplength);

if(value > 0)
{
    value = 1;
}

@P.y = fit01(value, min, max);
```



sources / further reading:
- [Nuke Wave Expressions - Cameron Carson](https://www.cameroncarson.com/nuke-wave-expressions)
- [List of Periodic Functions - Wikipedia](https://en.wikipedia.org/wiki/List_of_periodic_functions)


