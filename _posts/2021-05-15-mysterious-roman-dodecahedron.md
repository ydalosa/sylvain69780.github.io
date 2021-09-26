---
layout: default
title: "Mysterious Roman dodecahedron"
tags: shadertoy
---
# Mysterious Roman dodecahedron

While visiting the Lugdunum Museum at Lyon (France) I was shocked by a very strange artifact !  
 
![Lyon Roman dodecahedron](https://sylvain69780.github.io/assets/images/roman_dodecahedron.jpg)  

>A Roman dodecahedron or Gallo-Roman dodecahedron is a small hollow object 
made of copper alloy which has been cast into a regular dodecahedral shape: 
twelve flat pentagonal faces, each face having a circular hole of varying 
diameter in the middle, the holes connecting to the hollow center. 
Roman dodecahedra date from the 2nd to 4th centuries AD.  

Some people suggested it is a simple tool to make gloves because they are found only on the north of Europe, I prefer to think it's a kind of mystic object. We will probably never know !

There is several ways to get the SDF for a dodecahedron for Ray Marching. The Shadertoy user DjinnKahn created a shader that I was able to understand (so probably you will as well !). It is based on space "folding".

Folding "folds" a whole range of coordinates in another one. At the end we have only a subset of cartesians coordinates. The "ABS" function for example makes disappears all negative values that get "folded" to positive values. Before the "folding" operation, it may be interesting to get an id of the original domain of the point, in order to create variations while still calculating the shapes only once.

In 2D, If you expect to have an object with their "folded coordinates" out of the folded range of coordinates (for exemple an object with negative coordinates after an ABS), obviously there is not a single chance to have it drawn. But if you take care to well position it he will be drawn many times !

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
- R3 is the rotation needed to align with Icosahedron's faces
- R4 is the rotation needed to align with Dodecahedron's faces.

It is noticable that this is an hollow figure as this is a sphere elongated in 2 directions covering the face.

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

I also tried to create shapes with x3 and X5 polar symetry respectiverly 20 around the icosahedron vectrices and 12 around the faces midpoint. It seems simple to create many stellations, and virus like shapes.

<iframe width="320" height="180" frameborder="0" src="https://www.shadertoy.com/embed/Nd3GWf?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>  

<iframe width="320" height="180" frameborder="0" src="https://www.shadertoy.com/embed/Ns3Gzs?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>


To understand better how folding works, there is a very nice video of The Art of Code about "folding" 2D space to draw a nice Koch fractal !

I also noted some nice Celtic patterns done by Fabrice that may be used as a background.

At the end, I published this shader and had a lot of fun reading the comments !  

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/Nsd3Wl?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>  
  
References :

- [Roman dodecahedron - Wikipedia](https://en.wikipedia.org/wiki/Roman_dodecahedron)
- [The Roman Dodecahedron - An ancient mystery solved?](https://youtu.be/poGapxsanaI)
- [Wythoff construction on Shadertoy](https://www.shadertoy.com/results?query=tag%3Dwythoff).
- [icosahedronal symmetry - DjinnKahn](https://www.shadertoy.com/view/Mly3R3).
- [Icosahedron Weave - DjinnKahn](https://www.shadertoy.com/view/Xty3Dy)
- [Polyhedral Weave - Shane](https://www.shadertoy.com/view/MttczH)
- [Day 19 - Virus - jeko](https://www.shadertoy.com/view/WlKGRW)
- [Shader Coding: KIFS Fractals explained! - The Art of Code](https://youtu.be/il_Qg9AqQkE)
- [triskel (255 chars) - FabriceNeyret2](https://www.shadertoy.com/view/XlVXRW)
- [Celtic knot 2 - FabriceNeyret2](https://www.shadertoy.com/view/ld2BDy)
- [IQ Distance functions](https://www.iquilezles.org/www/articles/distfunctions/distfunctions.htm)
- [Pourquoi on ne Sait Rien de Cet Objet ? (en360s) - French - Poisson Fécond](https://youtu.be/JUcOBNReH2c)
- [FOLD & CUT - A Thesis - MICHAEL WILLIAM GIOFFREDI](https://core.ac.uk/download/pdf/154406344.pdf)
- [Kaleidoscopic (escape time) IFS - knighty](http://www.fractalforums.com/sierpinski-gasket/kaleidoscopic-(escape-time-ifs)/)
- [Polyhedras with control - Knighty](https://www.shadertoy.com/view/MsKGzw)
