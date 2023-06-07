---
title: "Alembic"
draft: false
tags:
- houdini
---

## Work with the Alembic Hierarchy

### Building Path Hierarchy

You can use the `[ ] Build Hierarchy From Attribute` option on the `Alembic ROP` to manage how geometry gets organized in an alembic file and rebuilt in e.g. Maya. the node expects a primitive attribute usually called `@path`.

//primitive wrangle

```C
s@path = "/Group/GeoNodeInMaya/ShapeNodeInMaya"
```

This is similar to how [[notes/USD |USD]] hierarchy is defined with `@name`.

### Path Attribute to Groups

A lot of the time you want to have access to the path as groups:

//primitive wrangle

```C
string name[] = split(s@path, "/");

foreach (string s; name){
    setprimgroup(0, s, @primnum, 1, "set");
} 
```

For more snippets check out [[notes/VEX Snippets |VEX Snippets]].

This creates groups for every segment of the path which can be a lot. To deal with them you can use wildcards in pretty much all fields that take group names.

Say you have a few paths that also include some material information:

- /GEO/Exterior/Glass/GEO
- /GEO/Interior/Glass/GEO
- /GEO/Interior/Leather/GEO

You can use that to assign a glass material to the relevant parts by using `*Glass*` in the group field.

## Hiccups

### Vertex Colors to Maya

Make sure to:
- not have any degenerate prims or loose points by using a `clean` node
- have `Cd` as a vertex attribute not point or prim

"Sometimes" this also helped:
- Use Hierarchy from Path option (could be any hierarchy)
- Create a 4 float `Cd` that also has an alpha channel
- Qualifier type has to be color (Clr) see below

![[notes/images/4fltColor.png]]

## Transforms & Instances

WIP

---

sources / further reading:
- [Exporting vertex colors (and other data) from Houdini to Maya - Toadstorm](https://www.toadstorm.com/blog/?p=240)
- [SOLVED: Alembic vertex colors export for Maya](https://www.sidefx.com/forum/topic/38939/?page=1#post-178401)
- [Houdini to Maya alembic: can't pass Cd SOLVED](https://www.sidefx.com/forum/topic/84355/?page=1#post-364666)
- [Exporting alembic with instances is bigger file size?!](https://www.sidefx.com/forum/topic/84766/)

