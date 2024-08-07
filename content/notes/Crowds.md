---
title: Crowds
draft: false
tags:
  - houdini
  - cfx
  - fx
---
The following notes only cover crowds with rigged agents, which are evaluated in real time during the simulation. Simple setups with instanced geometry caches are not covered here but may just as well be a viable solution.

## Agents

Every crowd instance is an agent.

### Agent Definition

An agent definition typically consists of a rig, one or more agent layers, which can hold different geometry variations or extra geometry and one or more agent clips, which store different animations for the rig.

To start creating an agent definition you need the `agent` SOP.

In the agent SOP you have multiple options to bring in your rig. The most straightforward way is to use a character rig and bring in a `Mocap Biped 3` character that ships with Houdini. This one is especially useful because it comes with many different animation clips which you can use.

I usually start by bringing in a t-posed character rig and name the clip "rest". 

### Geometry Variations > Agent Layers

Agent layers are used to store different geometry variations in the same base agent. You can for example attach different faces, hairstyles, clothing or other accessories to the agent definition, which can be selected or randomized with attributes later on. To attach something to you agent you can use the `agent layer` SOP.

##### Attaching Rigid Geometry (Accessories, Weapons etc.)

I like the old `Agent Layer` HDA more then the new one when it comes to attaching rigid geometry, which is already placed correctly (hat, sword etc.), because it has the option `Keep Position When Attaching`.

![[notes/images/agentlayerbindings.png]]

Anything you want to attach, has to have a name attribute. You can pin it to the rig by choosing a bone which is used to transform the geometry correctly. See example below:

![[notes/images/agent_layer_assign.png]]

If you want to add something to your base geometry, make sure to activate `Source Layer` under `Use Existing Shapes` and set it to whatever you chose to name your default layer.

##### Attaching Deforming Geometry (Clothes etc.)

To attach clothes or other geometry that has to be deformed correctly you can leave the `Transform Name` empty and select a shape deformer. 

![[notes/images/agentlayerbindingsshapedeform.png]]

For this to work the geometry has to be bound to the rig. When using the `Mocap Biped 3` I just crack the HDA open and bind the cloth geo to the rig using the shelf tools.

![[notes/images/agentlayercapture.png]]

Depending on your geo you may also get away with attribute transferring the `boneCapture` attribute from you t-posed character after `agent unpack`-ing them.

### More Animations > Agent Clips

To give your agent more animations you can use the `Agent Clips` SOP. 
You can provide the node with different datatypes:
- Character Rig
- FBX
- File
- CHOP
- USD
- Motionclip

When working with the default `Mocap Biped 3` I tend to create many different ones on OBJ level with different clips applied. Then I point the agent clips node to those OBJ objects. 

![[notes/images/agentclips_mocapbiped3.png]]

Make sure to set the start / end frame correctly to avoid unnecessary handles. This also comes in handy later when setting up custom looping and blending.

To prepare you agents for locomotion in a simulation you should disable `Inplace Animation` on the mocap biped 3 and enable `Convert to In-Place Animation` on the agent clips SOP. More on locomotion further down.

### Debugging your Agent

To visualize a specific agent (correct layers & clip) you can use the `agent edit` SOP. 

To preview a crowd and randomize different aspects you can use the `crowd source` SOP in combination with `crowd assign layers`.

## Animation

There are different ways to set up and use animation clips to create a crowd. Depending on your usecase you might get away with simple in-place animation or constant forward speed.

### In Place Animation

WIP

### Locomotion

WIP

### Foot Locking > Agent Prep

WIP

### Clip Transitions

The `Agent Clip Transition Graph` SOP lets you define, which and how these clips can blend between each other. There is a automatic function, which does a fairly good job a lot of the time. You can as well filter some of those automatic connections out by hand or create new ones that it didn't pick up by itself. I found the output of the automatic mode too confusing to work with when it comes to working with lots of clips: >15. I often end up just it up manually to be safe.

##### Dealing with Mirrored Clips Automatically

In this example I only set up 3 transitions by hand and the wrangle connects each mirrored version accordingly. It just assumes that all the timings and blend durations etc. are identical for the base and the mirrored clip.

![[notes/images/transitionmirror.png]]

// point wrangle "clipname_to_name"

```C
s@name = s@clipname;
```

// primitive wrangle "create_mirror_connections"

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

### Looping & Blending > Clip Properties 

You can use the `Agent Clip Properties` SOP to specify which clip can loop and how long/where to blend to avoid jumps. This creates a point per clip, which gets fed into the DOP crowd object. 

##### Dealing with Mirrored Clips Automatically 2

I use the following snippet combined with the one above to manage mirrores clips somewhat automatically.

![[notes/images/mirror properties.png]]

It's important that it has access to the tranistion graph output via the second wrangle input.

As before we also need to create a `name` attribute based on the clipname.

// point wrangle "create_mirror_properties"

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

### Blend Clips Together > Transform Groups

WIP

## Adjusting Clips using KineFX

### Mirroring

The `Rig Mirror Pose` SOP allows you to flip your clips and essentially double the amount of animations you have practically for free.

It doesn't work all the time because you need to provide it some kind of rest post / somewhat symmetrical centered frame of your clip.

![[notes/images/rigmirrorpose_crowd.png]]

You can feed the updated `Motion Clip` back in an `Agent Clip` SOP.

### On the Fly IK Chains

This allows you to easily manipulate and extend your animation library!

- Drop down a `Agent Animation Unpack` SOP
- time freeze and blast out any bones you want to add an animation to
- animate with a `Rig Pose` SOP
- Use the `IK Chains` SOP to blend in the animation
	- make sure to enable `Match By Name`
- Drop down a `Motion Clip` and feed it back to an `Agent Clip` node to create a new clip

![[notes/images/kinefx_ikchains_oncrowd.gif]]
## Simulation

WIP

### DOP Setup

WIP

### Triggers

WIP

### Forces

WIP

### Ragdolls

WIP

### Partial Ragdolls

WIP

### Vellum

WIP

## Post Sim Tweaks

### Agent Look At

You can make agents look at stuff after having already simulated everything! That is amazing and very simple to set up. You just need a `Agent Look At` SOP. Pipe in your crowd cache and you are good to go. You can use it in Simulation or in Live mode. If you don't simulate any turns you just directly modify the skeletons rotation to make each agent look at a target. Might have some snappy unexpected behavior but is usually good enough.

## Rendering

### Solaris, Karma & Hydra Delegates

##### Texture Variations in Solaris

1. **Karma CPU (VEX Shaders)**

For VEX Shaders you can modify many shader graph parameters using the `Material Variation` LOP. This way you can randomize or write custom logic in a vex snippet to define e.g. which path your diffuse texture is supposed to come from.

![[notes/images/assign_different_textures_crowd_karmavex.png]]

- the **index** variable  is a unique id per element 
- the **value** variable  is used to write back to the parameter specified under "Name"

2. **Karma XPU (MTLX Shaders)**

As of now (Houdini 19.5) it's not possible to get access to the filepath parameters in the `MTLX Image` node. The only node that can be accessed is the `MTLX Geometry Property Value`. Unfortunately you can't feed the `MTLX Image` node any string inputs. That's why you have to load in all your textures in separate nodes and vary the assignment with some switch logic based on your geompropvalue input. 

I usually assign different point attributes to the packed agent crowd in SOPs and read them in the shader and don't even bother using the `Material Variation` node.

![[notes/images/geompropvalue_shadersetup_crowd_matvariation.png]]

##### Targetting Sub-Geometry in Solaris

Let's say you have different agents with different layers. Some of those layers include extra geometry like a hat that requires a different shader than the main agent body. In OBJ / Mantra world you would have assigned the correct shader using material style sheets.

In USD you can't easily access sub-geometry when you are using `Instanced SkelRoots` (which is the default and recommended setting when working with crowds / SOP Crowd Import). If you wanted to assign specific shaders to different parts of your agent you would have to import the crowd as `SkelRoots` (not instanced) which you should probably only ever do for certain hero agents to not run out of RAM.

What now? 

You can do this via collections! (a USD thing) The setup isn't very straight forward though and I wouldn't have been able to get it to work without [this](https://www.sidefx.com/forum/topic/81706/?page=1#post-362705) forum post explaining how to do it.

The setup:

I want to assign the "hat" geometry a different shader then the rest of the agents. As explained above normal assignment using e.g. wildcards doesn't work:

![[notes/images/Pasted image 20231106143329.png]]

1. Drop down a `SOP Crowd Import` LOP and point it to your crowd
2. Create a `Collection` LOP
	1. give it a name
	2. check the `Allow Instance Proxies in Collection` toggle under options
	3. use the `%reference` expression to point to your geometry

```C
%reference:/crowd/agentdefinitions/agent/shapelibrary/hat
```

![[notes/images/Pasted image 20231106142401.png]]

3. Create a material library node and build some shaders
4. Use the `Assign Material` LOP to assign the material based on the collection name
	1. make sure to use `%yourCollectionName` 
	2. set the Method to `Collection Based`
	3. point the `Path` to your crowd object

![[notes/images/Pasted image 20231106142535.png]]

This should work in all hydra delegates (tested in Karma & Redshift).

![[notes/images/Pasted image 20231106143438.png]]

### Mantra & Third Party
##### Material Style Sheets

I didn't bother writing anything down because Mantra and Style Sheets are pretty much gone with USD and Solaris being more and more adopted. The [Zombies for Everyone](https://www.sidefx.com/tutorials/zombies-for-everyone-quick-intro-to-crowds/) course chapters 20 - 22 give you a good idea of the basics.

---

sources / further reading:
- [Zombies for Everyone | Quick Intro to Crowds - SideFX](https://www.sidefx.com/tutorials/zombies-for-everyone-quick-intro-to-crowds/)
- [Intro to Crowds - SideFX](https://www.sidefx.com/tutorials/intro-to-crowds/)
- [🚶🏻‍♂️ Crowds in Houdini Tutorials - Juanjo Martínez](https://www.youtube.com/playlist?list=PLyzn6-dYuCbFby6Ynlk5ziOM-YsTRw68K)
- [Crowds - cgwiki](https://www.tokeru.com/cgwiki/HoudiniCrowd.html)
- [Targetting sub-geometry in Solaris in a crowd sim - SideFX Forum](https://www.sidefx.com/forum/topic/81706/?page=1#post-362705)
- [Be in Control - Crowds and KineFX | Mikael Pettersen | Houdini 18.5 HIVE - SideFX](https://www.youtube.com/watch?v=1cLPYWj4Lqk)