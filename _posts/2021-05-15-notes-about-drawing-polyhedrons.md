---
layout: default
title: "Notes about drawing polyhedrons"
tags: shadertoy
---
# I'am fond of Polyhedra !

>In geometry, a polyhedron (plural polyhedra or polyhedrons) is a three-dimensional shape with flat polygonal faces, straight edges and sharp corners or vertices.  

Drawning polyhedra is quite cool, and these 3D shapes have something facinating.
Everybody knows the CUBE that is very commonly seen in our daily life is still very usefull !

First thing interesting is that there is only 5 regular convex polyhedra. This is just clear when one try to enumerate all possibilities to assemble triangle, square or pentagonal faces to build a volume.

References :  

- [Polyhedron - Wikipedia](https://en.wikipedia.org/wiki/Polyhedron)
- [Platonic solid - Wikipedia](https://en.wikipedia.org/wiki/Platonic_solid)
- [5 Platonic Solids - Numberphile](https://youtu.be/gVzu1_12FUc)
- [Les 5 solides de Platon - Micmaths](https://youtu.be/eDsFmYur9Yo) (French)
- [PuzzlingWorld By Stewart T. Coffin](https://johnrausch.com/PuzzlingWorld/contents.htm)

# Drawing Polyhedra using rasterization

On this page I focus on the Ray Marching method to render images, because this is what is mainly used on Shadertoy. Before that, let's have a look about how to do it using the more common rasterization method. As these figures have flat faces, this is obviously appropriate.

Using the IQ's Rasterizer shader, I learnt more about how to declare a Mesh object, that formally declare all the faces of our figure.

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/ftf3zn?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>

References :  

- [Scratch a Pixel free book](https://www.scratchapixel.com/lessons/3d-basic-rendering/introduction-polygon-mesh)
- [IQ rasterizer](https://www.shadertoy.com/view/4slGzn) 
- [Fabric Neyret Nesh loading tool](https://www.shadertoy.com/view/Wsy3DG)

# Cube

The exact SDF for a cube is simple, and there is an IQ's video on how to get it by yourseft.
Even more simple but not exact is to just take the MAX of the absolute value of each point coordinates ! (fBoxCheap in Mercury library)

References :

- [Ray marching starting point](https://shadertoy.com/view/WtGXDD) from BigWings, who precisely is drawing a cube.
- [Box function on IQ's SDF page](https://iquilezles.org/www/articles/distfunctions/distfunctions.htm)
- [Mercury library](http://mercury.sexy/hg_sdf/) 
- [Mercury library unofficial port on Shadertoy](https://www.shadertoy.com/view/Xs3GRB) 

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

## Mysterious Roman dodecahedron

While visiting the Lugdunum Museum at Lyon (France) I learnt that 2000 years ago, people were already fascinated by the shape of the Dodecahedron !  
 
![Lyon Roman dodecahedron](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e2/Dod%C3%A9ca%C3%A8dre_romain_au_Mus%C3%A9e_gallo-romain_de_Fourvi%C3%A8re.jpg/487px-Dod%C3%A9ca%C3%A8dre_romain_au_Mus%C3%A9e_gallo-romain_de_Fourvi%C3%A8re.jpg)  

>A Roman dodecahedron or Gallo-Roman dodecahedron is a small hollow object made of copper alloy which has been cast into a regular dodecahedral shape: twelve flat pentagonal faces, each face having a circular hole of varying diameter in the middle, the holes connecting to the hollow center. Roman dodecahedra date from the 2nd to 4th centuries AD.  

There is several ways to get this figure. The Shadertoy user DjinnKahn created a shader that I was able to understand. It is based on a space "folding" function allowing to also draw the spheres on the corners of the Dodecahedron.

Folding "folds" a whole range of coordinates in another one. This is called "folding" because at the end we have only manage a subset of cartesians coordinates. The "ABS" function for example makes disappears all negative values that get "folded" to positive values. Before the "folding" operation, it is interesting to get an id of the original domain, in order to create variations while still calculating the shapes only once.

There is a very nice video of The Art of Code about "folding" 2D space to draw a nice Koch fractal !

I also noted some nice Celtic patterns done by Fabrice that may be used as a background.

References :

- [Roman dodecahedron - Wikipedia](https://en.wikipedia.org/wiki/Roman_dodecahedron)
- [Wythoff construction on Shadertoy](https://www.shadertoy.com/results?query=tag%3Dwythoff).
- [icosahedronal symmetry - DjinnKahn](https://www.shadertoy.com/view/Mly3R3).
- [Icosahedron Weave - DjinnKahn](https://www.shadertoy.com/view/Xty3Dy) of the same author is a gold mine, just have to dig !
- [Day 19 - Virus - jeko](https://www.shadertoy.com/view/WlKGRW)
- [Shader Coding: KIFS Fractals explained! - The Art of Code](https://youtu.be/il_Qg9AqQkE)
- [triskel (255 chars) - FabriceNeyret2](https://www.shadertoy.com/view/XlVXRW)
- [Celtic knot 2 - FabriceNeyret2](https://www.shadertoy.com/view/XlVXRW)

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