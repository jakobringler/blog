---
title: "Velocity Fields"
draft: false
tags:
- houdini
---

## Advanced Velocity Fields

### Calculate Potential Flow with VDBs

![[notes/images/potential_flow_setup.png]]
You can use the `VDB Potential Flow` node to predict how air would move around an object. You can set the direction the 'wind' is coming from with the `Background Velocity` parameter on the node. To make the velocities converge behind the object you can use two cross product operations to cross the velocity field with the gradient of the sdf twice! 

//content of: doublecross_with_gradient - Volume VOP
![[notes/images/potential_flow_vop.png]]
Check out Mats video on the topic: [Houdini Tutorial: Advanced Velocity Fields / Part One - Mats](https://www.youtube.com/watch?v=K0cNvpXujmk)

### Strange Attractors

![[notes/images/strangeattractors.png]]

might be useful for some magic FX.

I put them on their own page [[notes/Strange Attractors |here]].

### Magnetic Fields

![[notes/images/Pasted image 20230706174615.png]]
- in the top left 3 points are created which have a `charge` attribute ranging from -1 to 1 resembling positive and negative magnets
- all the purple nodes setup the velocity field
- everything green is just for visualization

// volume wrangle

```C
int numCharges = npoints(1);
vector direction = set(0,0,0);

for( int i = 0; i < numCharges; i++ )
{
    vector chargePosition = point(1, "P", i);
    float chargeValue = point(1, "charge", i);
    
    vector chargeinfluence = (( chargePosition - v@P ) * chargeValue ) / pow(length(chargePosition - v@P), chf("power")); // set power to 3 as default
    direction += chargeinfluence;
}

direction = direction / numCharges;

v@vel = direction;
```

---

sources / further reading:
- [Houdini Tutorial: Advanced Velocity Fields / Part One - Mats](https://www.youtube.com/watch?v=K0cNvpXujmk)
- [Houdini Algorithmic Live [#021] - Magnetic Field Visualization -  Junichiro Horikawa](https://www.youtube.com/watch?v=pnfFbF-60qw)