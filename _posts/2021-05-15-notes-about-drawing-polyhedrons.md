---
layout: default
title: "Notes about drawing polyhedrons"
tags: shadertoy
---

Drawning polyhedrons is quite cool, and these 3D shapes have something facinating, I would make an exception for the CUBE that is very commonly in our daily life, but still very usefull !

First thing interesting is that there is only 5 regular polyhedrons [Platonic solid](https://en.wikipedia.org/wiki/Platonic_solid). This is just clear when one try to enumerate all possibilities to assemble triangle, square or pentagonal faces.

References :

- [5 Platonic Solids - Numberphile](https://youtu.be/gVzu1_12FUc)
- [Les 5 solides de Platon - Micmaths](https://youtu.be/eDsFmYur9Yo) (French)

Let's start with [Ray marching starting point](https://shadertoy.com/view/WtGXDD) from BigWings, who precisely is drawing a cube.

I first looked at [IQ's SDF page](https://iquilezles.org/www/articles/distfunctions/distfunctions.htm) and [Mercury library](http://mercury.sexy/hg_sdf/) and it's unofficial [port on Shadertoy](https://www.shadertoy.com/view/Xs3GRB)

The more classical way to do 3D is to define a [Mesh](https://www.scratchapixel.com/lessons/3d-basic-rendering/introduction-polygon-mesh).  
IQ did it in a pixel shader [here](https://www.shadertoy.com/view/4slGzn) and Fabrice a tool to load meshes encoded in images [here](https://www.shadertoy.com/view/Wsy3DG). 

# Cube

The exact SDF is simple, and there is an IQ's video on how to get it by yourseft.
Even more simple but not exact is to just take the MAX of the absolute value of each point coordinates ! (fBoxCheap in Mercury library)

- IQ's [box fonction](https://iquilezles.org/www/articles/distfunctions/distfunctions.htm) 

# Octahedron 

I tried to understand the Cheap IQ function below using [Desmos](https://www.desmos.com/calculator/ovavcqosu8)  
This is derived from the distance to a plan, that is dot(p,n) + h (on the same page)  
Here the plan normal n is vec3(1), to get it normalized there is this multiplication by 0.57735027 that is 1/sqrt(3).  
By taking the absolute value of each coordinate, the function need to take care of only 1 plan instead of 8.  
If I understand well, the distance is not too much over estimated, the maximum is sqrt(3) times the euclidian distance.  

```c
float sdOctahedron( vec3 p, float s)
{
  p = abs(p);
  return (p.x+p.y+p.z-s)*0.57735027;
}
```

# Icosahedron and Dodecahedron 
## Drawing Icosahedron and Dodecahedron Using a mesh
Using the IQ's Rasterizer shader, I learnt more about how to declare a Mesh object.

Let's use a mesh to declare our Dodecahedron.

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/ftf3zn?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>


## Drawing Druidic (previously assumed Roman) Dodecahedron
While visiting the Lugdunum Museum at Lyon (France) I learnt that Celtic druids already were fascinated by the shape of the Dodecahedron !  
 
![Celtic Dodecahedron](https://static.vici.org/cache/268x268-6/uploads/dode_cae_dre_romain_au_muse_e_gallo_romain_de_fourvie_re.jpg)  
[There is only some suppositions about the use the Druids was making of them.](http://www.celticnz.co.nz/Dodecahedron/Decoding%20the%20Druidic%20Dodecahedron1.html)  

I spent some time to search on Shadertoy for examples, some are very complex and using [Wythoff construction](https://www.shadertoy.com/results?query=tag%3Dwythoff). The Shadertoy user **mattz** is a kind of expert on it.

I found a simple exact SDF from [icosahedronal symmetry - DjinnKahn](https://www.shadertoy.com/view/Mly3R3). Even better, this is based on a space "folding" function, allowing to also draw the spheres on the corners of the Dodecahedron !

[Icosahedron Weave ](https://www.shadertoy.com/view/Xty3Dy) of the same author is a gold mine, just have to dig !

[Day 19 - Virus - jeko](https://www.shadertoy.com/view/WlKGRW) uses a pIcosahedron function to create again visus like shapes. Seems using **Knighty** function.

Time to make some experiments to understand how this folding technic is working.
Folding makes "fold" a whole range of coordinates in another one. This is called "folding" because at the end we have only manage a subset of cartesians coordinates. The "ABS" function for example makes disappears all negative values that get "folded" to positive values. Before the "folding" operation, it is interesting to get an id of the original domain, in order to create variations while still calculating the shapes only once.

There is a very nice video of The Art of Code about how this is used and at the end to draw a nice fractal !

<iframe width="560" height="315" src="https://www.youtube.com/embed/il_Qg9AqQkE?controls=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Polyhedrons as Voronoi's cell for 3D lattices

Tiling space is usually done using cubes, but it is also possible using truncated octahedrons and rhombic dodecahedrons.

Here Shadertoy meets the Christalography science, where these lattices are named CC, FCC, BCC. [Cubic crystal system - Wikipedia](https://en.wikipedia.org/wiki/Cubic_crystal_system)

* Primitive cubic (abbreviated cP and alternatively called simple cubic)
* Body-centered cubic (abbreviated cI or bcc)
* Face-centered cubic (abbreviated cF or fcc, and alternatively called cubic close-packed or ccp)

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/llfGRj?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>
  
**[CC / FCC / BCC Lattices ](https://www.shadertoy.com/view/llfGRj)**  

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/WdXBR8?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>
  
**[Spalmer - Rhombic Dodecahedron Voxels](https://www.shadertoy.com/view/WdXBR8)**  

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/lttGDn?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe> 
  
**[Shane - Lattice Framework](https://www.shadertoy.com/view/lttGDn)**  

> Here *Shane* shows some great tricks to short code the map fonction, taking all the advandages of the geomery of the lattice, combining distances to create more shapes. No UV coordinates are used for textures, only triplanar mapping using hit point coordinates and the normal. Powerfull. Beautyfull.


** DR2 Icosahedral Variations

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/MdXfWS?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>
  
> This is used by several creators to create Virus like shapes.