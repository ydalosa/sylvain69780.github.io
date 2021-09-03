---
layout: default
title: "I'am fond of Polyhedra"
tags: shadertoy
---
# I'am fond of Polyhedra !

>In geometry, a polyhedron (plural polyhedra or polyhedrons) is a three-dimensional shape with flat polygonal faces, straight edges and sharp corners or vertices.  

Drawning regular polyhedra is quite cool, and these 3D shapes have something facinating.
Everybody knows the CUBE that is very commonly seen in our daily life is still very usefull !

First thing interesting is that there is only 5 regular convex polyhedra (Platonic solids). Just try to enumerate all possibilities to assemble triangle, square or pentagonal faces to build a volume and you will get an idea of the demonstration.

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
- [Rasterizer - Object - IQ](https://www.shadertoy.com/view/4slGzn) 
- [raytrace geometry images as vol - Fabric Neyret](https://www.shadertoy.com/view/Wsy3DG)

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

References :

- [3D distance functions - IQ](https://www.iquilezles.org/www/articles/distfunctions/distfunctions.htm) 

# Tiling space with Polyhedra

Tiling space is usually done using cubes, but it is also possible using truncated octahedrons and rhombic dodecahedrons.

Here Shadertoy meets the Christalography science, where these lattices are named CC, FCC, BCC. 

- Primitive cubic (abbreviated cP and alternatively called simple cubic)
- Body-centered cubic (abbreviated cI or bcc)
- Face-centered cubic (abbreviated cF or fcc, and alternatively called cubic close-packed or ccp)

In "Lattice Framework", *Shane* shows some great tricks to short code the map fonction, taking all the advandages of the geomery of the lattice, combining distances to create more shapes. No UV coordinates are used for textures, only triplanar mapping using hit point coordinates and the normal. Powerfull. Beautyfull.

Refereneces

- [Cubic crystal system - Wikipedia](https://en.wikipedia.org/wiki/Cubic_crystal_system)
- [CC / FCC / BCC Lattices -paniq](https://www.shadertoy.com/view/llfGRj)  
- [Spalmer - Rhombic Dodecahedron Voxels](https://www.shadertoy.com/view/WdXBR8)
- [Shane - Lattice Framework](https://www.shadertoy.com/view/lttGDn)  
  

