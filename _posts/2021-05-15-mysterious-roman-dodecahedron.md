---
layout: default
title: "Mysterious Roman dodecahedron"
tags: shadertoy
---
# Mysterious Roman dodecahedron

While visiting the Lugdunum Museum at Lyon (France) I was shocked !  
 
![Lyon Roman dodecahedron](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e2/Dod%C3%A9ca%C3%A8dre_romain_au_Mus%C3%A9e_gallo-romain_de_Fourvi%C3%A8re.jpg/195px-Dod%C3%A9ca%C3%A8dre_romain_au_Mus%C3%A9e_gallo-romain_de_Fourvi%C3%A8re.jpg)  

>A Roman dodecahedron or Gallo-Roman dodecahedron is a small hollow object made of copper alloy which has been cast into a regular dodecahedral shape: twelve flat pentagonal faces, each face having a circular hole of varying diameter in the middle, the holes connecting to the hollow center. Roman dodecahedra date from the 2nd to 4th centuries AD.  

Some people suggested it is a simple tool to make gloves because they are found only on the north of Europe, I prefer to think it's a kind of mystic object. We will probably never know !

Gallo-Romans already know about the shape of the Dodecahedron ! 2000 years after, nothing changed, Shadertoy's users are fond of this figure

There is several ways to get the SDF for a dodecahedron for Ray Marching. The Shadertoy user DjinnKahn created a shader that I was able to understand (so you probably will !). It is based on a space "folding".

Folding "folds" a whole range of coordinates in another one. This is called "folding" because at the end we have only manage a subset of cartesians coordinates. The "ABS" function for example makes disappears all negative values that get "folded" to positive values. Before the "folding" operation, it is interesting to get an id of the original domain, in order to create variations while still calculating the shapes only once.

In 2D, If you expect to have an object with their "folded coordinates" out of the folding range of coordinates (for exemple an object with negative coordinates for an ABS value), obviously there is not a single chance to have it drawn. But if you take care to well position it he will be drawn many times !

In the DjinnKahn's shader, at the end the coordinates are bound between 3 **directions** from the origin.
```
// return value will be between these rays:
//    vertex:        vec3(0,A,B)
//    face midpoint: vec3(0,A,B)*(1./3.) + vec3(0,0,A)*(2./3.)
//    edge midpoint: vec3(0,A,B)*.5 + vec3(B,0,A)*.5  
```
These directions are **edges**  of the repetition domain.  
By rotating the coordinates to be aligned with one of these directions, and the origin transated on a point on a given direction, we can get some interesting coordinates relative to the faces of the solids.
DjinnKahn used this to get the exact distance to the edges and the faces of the icosahedron and the dodecahedron. He also managed to put some spheres over some well known positions.  

In the code below, we move the point "radius" units along a direction, and rotate to have coordinates alignes with the polyhedron faces.
- R3 is the rotation needed to align with a Icosahedron's face
- R4 is the rotation needed to align with a Dodecahedron's face.
It is noticable that this is an hollow figure as there is no "max" on the Z coordinate.

```
float GetDist(vec3 p) {
    float radius = 1.0 + .5 * sin(iTime);
    vec3 q = opIcosahedron( p );
    vec3 pIco  = R3 * (q - ICOVERTEX*radius);
    vec3 pDode = R4 * (q - ICOMIDFACE*radius);
//    float dist = sdSphere( vec3( max(pIco.x, 0.), max(pIco.y, 0.), pIco.z ), .1 );
    float dist = sdSphere( vec3( max(pDode.x, 0.), max(pDode.y, 0.), pDode.z ), .1 );
    return dist;
}
```

I also managed to create shapes with x3 and X5 polar symetry respectiverly 20 around the icosahedron vectrices and 12 around the faces midpoint. It seems simple to create many stellations, and virus like shapes.

To understand better how folding works, there is a very nice video of The Art of Code about "folding" 2D space to draw a nice Koch fractal !

I also noted some nice Celtic patterns done by Fabrice that may be used as a background.

References :

- [Roman dodecahedron - Wikipedia](https://en.wikipedia.org/wiki/Roman_dodecahedron)
- [The Roman Dodecahedron - An ancient mystery solved?](https://youtu.be/poGapxsanaI)
- [Wythoff construction on Shadertoy](https://www.shadertoy.com/results?query=tag%3Dwythoff).
- [icosahedronal symmetry - DjinnKahn](https://www.shadertoy.com/view/Mly3R3).
- [Icosahedron Weave - DjinnKahn](https://www.shadertoy.com/view/Xty3Dy)
- [Day 19 - Virus - jeko](https://www.shadertoy.com/view/WlKGRW)
- [Shader Coding: KIFS Fractals explained! - The Art of Code](https://youtu.be/il_Qg9AqQkE)
- [triskel (255 chars) - FabriceNeyret2](https://www.shadertoy.com/view/XlVXRW)
- [Celtic knot 2 - FabriceNeyret2](https://www.shadertoy.com/view/XlVXRW)

