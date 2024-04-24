---
title: "VEX Snippets"
tags:
- vex
- houdini
enableToc: true
---

## Wrangle Cheat Sheet

>I try my best to credit and link to any sources. That being said, some of those are pretty old and I have no idea where they came from.
>
>I recommend installing [Vex Snippet Library](https://github.com/dchow1992/Vex_Snippet_Library) to manage your own snippet collection. You can find all snippets formatted as .json files ready to be plugged into the library [here](https://github.com/jakobringler/H_VEX_snippets).

> [!tip] **Hot Tip:**
> 
> You can find the attribute wrangle node by just typing `aw` ;)

## Utilities

### Attribute based Probability Grouping
//point wrangle

```C
// adjust in min max to attribute min max
float norm = fit(@pscale, chf("in_Min"), chf("in_Max"), 0, 1);
// control distribution with ramp
float ramp = chramp("distribution", norm);
// set min max percentage to 0/1 in the beginning
// adjust for random strays
float max = chf("max_Percentage");
float min = chf("min_Percentage");
float rand = rand(@ptnum);
float percentage = clamp(fit01(ramp, min, max), 0, 1);

i@group_selected = rand<percentage;
```

### Angle between 2 Vectors
// point wrangle 

```C
float angle = degrees(acos(dot(normalize(vector1), normalize(vector2))));
```

### Attribute Min Max
// point wrangle

```C
float value;
float values[];
string attrname = "name";
float max_value;
float min_value;

for (int i=0; i<@numpt; i++)
{
	value = point(geoself(), attrname, i);
	append(values,value);
}

min_value = min(values);
max_value = max(values);

f[]@range;

@range[0] = min_value;
@range[1] = max_value;
```

### Attribute to String
// point wrangle

```C
s@name = "piece_" + itoa(i@class);
```

### Attribute Transfer
// point wrangle

```C
int posprim;
vector param_uv;
float maxdist = 10;
float dist = xyzdist(1,@P,posprim,param_uv,maxdist);
vector pos = primuv(1,"rest",posprim,param_uv);
v@rest = pos;
```

### Average Point Cloud Positions
// point wrangle

```C
vector value;
vector values[];

for (int i=0; i<@numpt; i++)
{
	value = point(geoself(), "P", i);
	append(values,value);
}
  
vector avgP = avg(values);

if(@ptnum>0)
{
	removepoint(geoself(), @ptnum);
} 

@P = avgP;
```

### Bias and Gain
// point wrangle

```C
function float bias(float val; float bias) 
{
    return (val / ((((1.0/bias) - 2.0)*(1.0 - val))+1.0));
}

function float gain(float val; float gain) 
{
    if(val < 0.5)
    {
        return bias(val * 2.0,gain)/2.0;
    }
    else
    {
        return bias(val * 2.0 - 1.0,1.0 - gain)/2.0 + 0.5;
    }
}

float val = pow(val, exp); 
```

> [!quote] **Sources:**
> 
> [Michael Frederickson's Tweet](https://twitter.com/mfrederickson/status/1523148417349816320)
> 
> [Alan Wolfe's Blog Post](https://blog.demofox.org/2012/09/24/bias-and-gain-are-your-friend/)

### Bounding Box
// point wrangle

```C
vector bbox = getbbox_size(0);
vector bbox_max = getbbox_max(0);
vector bbox_min = getbbox_min(0);
vector bbox_center = getbbox_center(0);
  
float xsize = getbbox_size(0).x;
float y_max = getbbox_max(0).y;
float z_min = getbbox_min(0).z;
```

### Camera Position and Direction
// point wrangle

```C
string cam = chs("cam");
matrix camXform = optransform(cam); 
vector cpos;
vector cdir;
vector cup;

cpos = cracktransform(0, 0, 0, {0,0,0}, camXform);
cdir = vtransform(cam,"space:world", {0,0,-1});
cup = vtransform(cam,"space:world", {0,1,0});

v@P = cpos;
v@N = cdir;
v@up = cup;
```

### Collision Check and Deintersection with SDF VDB
// point wrangle

```C
vector gradient = volumegradient(1, "surface", v@P); 
float surface = volumesample(1, "surface", v@P);

if(surface < chf("dist"))
{
	v@P += normalize(gradient) * abs(surface);
}
```

### Color Palette Refinement using 'Despill' Technique
// point wrangle

```C
// rotate hue
v@Cd = rgbtohsv(v@Cd);
@Cd.r += chf("rotate_Hue");
v@Cd = hsvtorgb(v@Cd);

// define color spread (value of 2 for Tolerance keeps input colors)

vector originalCd = v@Cd;

if (@Cd.g > (@Cd.b + @Cd.r) / 2 * (chf("Tol") * 2))
{
    @Cd.g = (@Cd.b + @Cd.r) / 2 * (chf("Tol") * 2);
}
else
{
    @Cd.g = @Cd.g;
}

// apply tint

@Cd += length(originalCd - v@Cd) * chv("Tint") * chf("Luma");

// rotate hue back
v@Cd = rgbtohsv(v@Cd);
@Cd.r -= chf("rotate_Hue");
v@Cd = hsvtorgb(v@Cd);
```

> [!quote] **Sources:**
> 
> [Chris Turner's Twitter thread about Compositing Tricks](https://twitter.com/allexceptn/status/1439253876041998346)

### Crowd Transition Mirrored Clips
// primitive wrangle

```C
string mirrorsuffix = "_m";
string newclipname_a = s@clip_a + mirrorsuffix;
string newclipname_b = s@clip_b + mirrorsuffix;

int clip_a = nametopoint(0, s@clip_a);
int clip_a_m = nametopoint(0, newclipname_a);
int clip_b = nametopoint(0, s@clip_b);
int clip_b_m = nametopoint(0, newclipname_b);

if( clip_a_m != -1 )
{
    int prim = addprim(0, "polyline", clip_a_m, clip_b);
    int pA_clip_a = setprimattrib(0, "clip_a", prim, newclipname_a, "set");
    int pA_clip_b = setprimattrib(0, "clip_b", prim, s@clip_b, "set");
    int pA_blend = setprimattrib(0, "blend_durations", prim, f[]@blend_durations, "set");
    int pA_sync = setprimattrib(0, "sync_points", prim, f[]@sync_points, "set");
    int pA_regions = setprimattrib(0, "transition_regions", prim, f[]@transition_regions, "set");
}

if( clip_b_m != -1 )
{
    int prim = addprim(0, "polyline", clip_a, clip_b_m);
    int pA_clip_a = setprimattrib(0, "clip_a", prim, s@clip_a, "set");
    int pA_clip_b = setprimattrib(0, "clip_b", prim, newclipname_b, "set");
    int pA_blend = setprimattrib(0, "blend_durations", prim, f[]@blend_durations, "set");
    int pA_sync = setprimattrib(0, "sync_points", prim, f[]@sync_points, "set");
    int pA_regions = setprimattrib(0, "transition_regions", prim, f[]@transition_regions, "set");
}

if( clip_a_m != -1 && clip_b_m != -1 )
{
    int prim = addprim(0, "polyline", clip_a_m, clip_b_m);
    int pA_clip_a = setprimattrib(0, "clip_a", prim, newclipname_a, "set");
    int pA_clip_b = setprimattrib(0, "clip_b", prim, newclipname_b, "set");
    int pA_blend = setprimattrib(0, "blend_durations", prim, f[]@blend_durations, "set");
    int pA_sync = setprimattrib(0, "sync_points", prim, f[]@sync_points, "set");
    int pA_regions = setprimattrib(0, "transition_regions", prim, f[]@transition_regions, "set");
}
```

This is a useful snippet to setup correct transitions of mirrored clips based on the transition settings configured by hand for the base clips. To use this you have to create a name attribute on your points by copying the `clipname` attribute. You can find more [[notes/Crowds#Dealing with Mirrored Clips Automatically |here]].

### Crowd Clip Properties Mirrored Clips
// point wrangle

```C
string mirrorsuffix = "_m";
string clipname = s@clipname;
string newclipname = clipname + mirrorsuffix;

int clip_m = nametopoint(0, newclipname);
int clip_exists = nametopoint(1, newclipname);

if( clip_m == -1 && clip_exists != -1)
{
    int pt = addpoint(0, set(0,0,0));
    int pA_clipname = setpointattrib(0, "clipname", pt, newclipname);
    int pA_agent = setpointattrib(0, "agentname", pt, s@agentname);
    int pA_clipnamesrc = setpointattrib(0, "clipname_source", pt, newclipname);
    int pA_blend_a = setpointattrib(0, "blend_duration_after", pt, @blend_duration_after);
    int pA_blend_b = setpointattrib(0, "blend_duration_before", pt, @blend_duration_before);
    int pA_loop = setpointattrib(0, "enable_looping", pt, @enable_looping);
    int pA_gait = setpointattrib(0, "gait_speed", pt, @gait_speed);
    int pA_loopr = setpointattrib(0, "loop_range", pt, u@loop_range);
    int pA_sampler = setpointattrib(0, "sample_rate", pt, @sample_rate);
    int pA_start_time = setpointattrib(0, "start_time", pt, @start_time);
}
```

Read more [[notes/Crowds#Dealing with Mirrored Clips Automatically 2 |here]]
### Edgefalloff
// point wrangle

```C
if (@edgefalloff==1)
{
	int near[] = nearpoints(0,@P,chf("dist"));
	
	foreach (int pnt;near)
	{
		vector pntP = point(0,"P",pnt);
		float dist = fit(distance(pntP,@P),0,chf("dist")*2,1,-1);
		setpointattrib(0,"edgefalloff",pnt,dist,"set");
	}
}
```

### Expand Group Over Geo
// point wrangle

```C
int pc = pcopen(0, 'P', @P, chf('radius'), chi('maxpts'));

while (pciterate(pc) > 0)
{
	int currentpt;	
	pcimport(pc, 'point.number', currentpt);
	setpointgroup(0, 'group1', currentpt, 1);
}
```

### Extract Tranformation Matrix
// point wrangle

```C
vector P1 = point(0, "P", 0);
vector P2 = point(0, "P", 1);
vector up = {0,1,0};

vector X = normalize(P2-P1);
vector Z = normalize(cross(up, X));
vector Y = normalize(cross(X, Z));

vector P = P1 + (P2 - P1) / 2;

matrix transform = set(X, Y, Z, P);

setcomp(transform, 0, 0, 3); 
setcomp(transform, 0, 1, 3);
setcomp(transform, 0, 2, 3); 

4@transform = transform;
```

Have a look at [[notes/Matrix Operations |this note]] for more information on how it's used.

### Flow Vector around Geometry
// point wrangle

```C
 vector up = chv("up"); // usually 0, 1, 0 for Y  
 vector norm = normalize(v@N);  
 vector c1 = cross(norm, up);  
 vector c2 = cross(norm, c1);  
 v@revolve = c1;
 v@flow = c2;
```

### Follow Surface with Particles using the VDB Gradient
// point pop wrangle

```C
float surf = volumesample(1, "surface", v@P);
vector grad = volumegradient(1, "surface", v@P);

v@P -= normalize(grad) * surf;
```

### Frustum Culling Volume Fields in DOPs
// gas field wrangle

```C
string cam = chsop("cam");
vector ndcP = toNDC(cam,@P);
//vector camP = ptransform("space:camera",@P); //non perspective space if needed

float mult = 1;

if(chi('cull_x_negative')) mult *= ndcP.x > -ch('cam_x_neg') ;
if(chi('cull_x_positive')) mult *= ndcP.x < 1 + ch('cam_x_pos') ;
if(chi('cull_y_negative')) mult *= ndcP.y > -ch('cam_y_neg') ;
if(chi('cull_y_positive')) mult *= ndcP.y < 1 + ch('cam_y_pos') ;
if(chi('cull_z_negative')) mult *= ndcP.z < ch('cam_z_neg') ;
if(chi('cull_z_positive')) mult *= ndcP.z > -ch('cam_z_pos') ;


//foreach( int i; string field; split(chs('fields'))){} //maybe loop over given fields, usually density is enough
f@density *= mult;
f@temperature *= mult;
f@flame *= mult;
@Cd *= mult;
```

> [!quote] **Sources:**
> 
> [Lewis Taylor's Talk on Volume Efficiency](https://vimeo.com/894363970)

Read more [[notes/Volumes#Frustum Rasterizing vs Frustum Culling SOPs vs Frustum Culling DOPs |here]]

### Group by N Connections
// point wrangle

```C
int n = chi("Neighbours");

if (neighbourcount (0, @ptnum) > n)
{
	setpointgroup (0, "grouped", @ptnum, 1);
}
```

### Gravity on Curves (Hanging Cables)
// point wrangle

```C
float stiffness = clamp(chf("stiffness"), 0, 0.99);
float u = @curveu * (1 - @curveu) * 4;
u = pow( 1 - u, (1 / (1 - stiffness)));

@P.y *= clamp(fit01(gradient, (1 - ch("gravity")), 1), 0, 1);
```

> [!quote] **Sources:**
> 
> [Chris Turner's Tweet](https://twitter.com/allexceptn/status/1488954032425213958)

### Helix from Line
// point wrangle

```C
  float freq = chf("freq");
  float amp = chf("amp");
  vector pos = @P;
  
  pos.x += sin(@P.y * freq) * amp;
  pos.z += cos(@P.y * freq) * amp;
  @P = pos;
```

### If Statements, Ternary Operations, Select Function
// point wrangle

```C
// long and most common version
if(value > threshold)
{
	value;
}
else
{
	0;
}

// Ternary
value = (value > threshold) ? value: 0;
// this does the same thing but as a oneliner 
// (condition) ? ValueIfTrue: ValueIfFalse

// Select Function (the fastest of the three when it comes to pure performance)
value = select( value > threshold, value, 0);
// select(condition, ValueIfTrue, ValueIfFalse)

// @swalsch pointed out on the cgwiki discord that in this case the absolutely fastest and most elegant way would be as follows:
value *= value>threshold;
// which is so elegant that I had to include it. (Only works if your False Value is 0 anyways)
```

### Inflate Geo and Avoid Intersections / Fake Collisions 
// point wrangle

This snippet is best used by running it inside a compiled for-each loop set to "by count". Make sure to recalculate the point normals of the geometry inside the loop before the wrangle!

```C
int handle;
int maxPoints = chi('maxPts');
float searchDist = ch('searchDist');
vector avgN;

int n[] = nearpoints(0, v@P ,searchDist,maxPoints);
avgN = v@N;
for( int ii = 0 ; ii < len(n) ; ii++ ) {
    int pt = n[ii];
    
    if( ii == 1 ){
        avgN = point(0, "N", pt);        
    }
    if( ii > 1 ){ 
        avgN += point(0, "N", pt);
    }
}
avgN /= len(n);

float dot;
dot = dot(@N,avgN);
dot = fit(dot,-.1,1,0,1);
@P += v@N * ch('stepSize') * dot;
```

> [!quote] **Sources:**
> 
> [Jacob Clark](https://www.linkedin.com/in/jclark2900/) shared this amazing setup on a houdini discord channel. Thanks for letting me post it!

### Isolate Overlapping Points
// point wrangle

```C
int near[] = pcfind(0, "P", @P, 0.0001, 2);

if(len(near) > 1)
{
	setpointgroup(0, "double", @ptnum, 1);
}
```

### Name Attribute for each Prim Group
// point wrangle

```C
string grps[] = detailintrinsic(0, 'primitivegroups');
foreach(string g; grps)
{
	addprimattrib(0, g, 123);
}
```

### Normalize @age
// point wrangle

```C
@normage = @age / @life;
```

### Orientation Template for Copy
// point wrangle

```C
@up = {0,1,0};
@orient = quaternion(maketransform(@N,@up));
vector4 rot_Y = quaternion(radians(ch('Y')),{0,1,0});
@orient = qmultiply(@orient, rot_Y);
```

### Point Density
// point wrangle

```C 
float maxdist = chf("maxdist");  
int maxpts = chi("maxpts");  
int points = len(nearpoints(0, @P, maxdist, maxpts));  
  
f@density = float(points) / maxpts;
```

### Ray to Surface SDF
// point wrangle

```C
@P -= volumegradient(1, "surface", @P) * volumesample(1, "surface", @P) 
```

### Remove Point by Condition
// point wrangle

[Mai Ao](https://twitter.com/aomai01) compared two point deletion methods, where method 1 gives a 15x speed increase over the traditional `removepoint()` function

1. group points first and blast group in another step

```C
float half_cone_rad = radians(chf("half_cone"));
@group_to_delete = acos(dot(@P, {0,0,1})) <= half_cone_rad;
```

2. removepoint()

```C
float half_cone_rad = radians(chf("half_cone"));

if(acos(dot(@P, {0,0,1})) <= half_cone_rad)
{
	removepoint(0, @ptnum);
}
```

> [!quote] **Sources:**
> 
> [Mai Ao's Tweet](https://twitter.com/aomai01/status/1514226273794641925/photo/1)

### Remove Point Percentage
// point wrangle

```C
int percentage = ch('percentage'); 

if(@ptnum % 100 < percentage)
{
	removepoint(0, @ptnum );
}
```

### Remove Point Percentage (ID aware)
// point wrangle

```C
if (haspointattrib(0, "id"))
{    
    if (rand(@id+chi('seed')+0.33)<chf('percentage'))
    {
        removepoint(geoself(), @ptnum);
    }    
}
else
    if (  rand(@ptnum+chi('seed')+0.33)<chf('percentage')) 
    {
        removepoint(geoself(), @ptnum);
    }
```

> [!quote] **Sources:**
> 
> Thanks Hannes!

### Rotate Vector
// point wrangle

```C
float angle = chf("angle");
vector axis = normalize(chv("axis"));

vector4 rot = quaternion(radians(angle), axis);
@P = qrotate(rot, @P);
```

### Sharpen Point Cloud
// point wrangle

```C
int handle = pcopen(0, "P", @P, chf("radius"), chi("maxpoints"));
v@P = pcfilter(handle, "P");
```

### Vector Flow on Objects
// point wrangle

```C
vector pos = v@P * chf("scale");
vector dir = curlnoise2d(pos + @Time * chf("time"));
matrix3 mat = dihedral(set(0,0,1), v@N);
dir *= mat;

v@v = dir;
```

### Velocity Tester
// point wrangle

Very useful to visualize how meshed fluids will move in the motion blur of a rendered frame.

```C
@v *=  chf('vScale'); // Should be 1 by default
float velMax = ch('max_vel'); // Should be a high number by default
float velLength = length(@v);

// Clamp Velocity
if(velLength > velMax)
    {
    @v = normalize(@v)*velMax;
    }
    
// Export Speed
if (chi("export_Speed")==1)
    {
    @speed = length(@v);
    }

// Test Velocity
@P += @v/$FPS*chf('velTester');
```

> [!quote] **Sources:**
> 
> Thanks Hannes!

### Wave Expressions

[[notes/WaveExpressions |Summary]] of different useful periodic functions like square or sawtooth

