---
layout: default
title: "Magic of Fold and Cut"
tags: shadertoy
---
# Magic of Fold and Cut

In june 2021, I was watching a awesome video of "The Art of Code on youtube" about light relecting and refracting on a gem stone.  

<iframe width="320" height="180" frameborder="0" src="https://www.shadertoy.com/embed/sllGDN?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>

The distance function used for this Gem stone is copied below. I appears like a magic trick, because all we have here are only ABS and mirroring "folding" expressions (explained in another video about the Koch fractal). At the end we get a "Rhombic Triacontahedron" !

```
float GetDist(vec3 p) {
    p.xz *= Rot(iTime*.1);
    float d = sdBox(p, vec3(1));
    
    float c = cos(3.1415/5.), s=sqrt(0.75-c*c);
    vec3 n = vec3(-0.5, -c, s);
    
    p = abs(p);
    p -= 2.*min(0., dot(p, n))*n;
    
    p.xy = abs(p.xy);
    p -= 2.*min(0., dot(p, n))*n;
    
    p.xy = abs(p.xy);
    p -= 2.*min(0., dot(p, n))*n;
    
    d = p.z-1.;
    return d;
}
```

The magic is related to the Wythoff construction, a method to generate the verticies of many polyhedrons from a tiling of the unit sphere with identical spherical triangles (triangles that are not drawn on a plane but on a sphere).

The sphere can only be tiled with triangles having angles that are fractions of PI. Let say PI/2 , PI/3, PI/5. Ah ah and the trick is if you add these angles you get (15+10+6)/30 = 31/30 of PI, that is impossible on a plan triangle if you remember your elementary school courses, but possible (and needed) on a spherical triangle ! You learnt that the sum of the angles of a triangle can't be greater than 180 degres. This is only true on a plan !

<iframe width="320" height="180" frameborder="0" src="https://www.shadertoy.com/embed/NsdXRr?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>  

Now the way to define our triangle is to get 3 plans that intersect at the origin, and having their dihedral angle equal to the angles of the triangle. Let's say PI/2 , PI/3, PI/5.

We can easily find 2 plans a and b that have a dihedral angle of PI/2, the xz and yz plans, making the folding operation on these plans a simple ABS(p.xy). Saying na, nb, nc are the normals to the plans a, b, c, in oder to get the third plan c, we need, nc as follows.

```
cos(PI/3) = 0.5 = dot(na,nc)
cos(PI/5) = cospin = dot(nb,nc)
```

As na = vec3(1,0,0) and nb = vec3(0,1,0) we get
nc.x = 0.5
nc.y = cos(PI/5)

We want to also the length of this normal to be 1, so 

```
nc.x² + nc.y² + nc.z² = 1  
nc.z = sqrt(1 - nc.x² - nc.y²)  
nc.z = sqrt(0.75 - cospin * cospin )  
```

This explains mainly the calculation below. I just missed something abouth the sign, probably it is adjusted to have the folding to work properly.

```
    float c = cos(3.1415/5.), s=sqrt(0.75-c*c);
    vec3 n = vec3(-0.5, -c, s);
```

# Why this generates a nice Gem stone at the end ?

It appears that the sphere is tiled with triangles that are, assembled 4 by 4, forming a rhombus around the P/2 angle. 
- These 4 triangles are coplanar because sharing a same corner around the Z axis.
- Other questions comes, like why can't I find any mention of this construction to get the Rhombic triacontahedron on Internet ? Would be happy to have more references to put below.

# How to get exact distance to this Gem Stone ?

Many comments are about the fact that this distance function generates sharp edges.
This is caused by the fact that the distance estimation is not exact near the edges, and it prevents to get soft edges by substracting a distance. The trick used by Bigwings was to just tweek the normal calculation function.
In order to get an exact distance, it seems suffisent to get a distance to the edges to add it to the "GetDist" distance estimation function.

This edge is the line connecting the intersection points of the c plan with the x and y axis.

Using Desmos I found the coordinates of the intersection points, and the equation to find the position of the edge.

<iframe src="https://www.desmos.com/calculator/qb6yvprhv1?embed" width="320" height="180" style="border: 1px solid #ccc" frameborder=0></iframe>

By adding these lines in the distance estimation function, we can get soft edges for the Rhomic Triaconthadedron.

```
float sdCapsule( vec3 p, vec3 a, vec3 b, float r )
{
  vec3 pa = p - a, ba = b - a;
  float h = clamp( dot(pa,ba)/dot(ba,ba), 0.0, 1.0 );
  return length( pa - ba*h ) - r;
}
```

```
    vec3 a = vec3(0.0,s/c,1.0);
    vec3 b = vec3(.5/c,0.0,1.0);
    vec2 line = vec2(.5,c);
    if ( dot(p.xy,line) > s ) d = sdCapsule(p,a,b,.0); 
```

<iframe width="320" height="180" frameborder="0" src="https://www.shadertoy.com/embed/NstSzH?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>

# Related References

Mattz's shader "Wythoff explorer" is a pure gem.  

If you look carfully to the comments and the graphical interface, it explains all about the Wythoff construction.

- [Wythoff explorer - mattz](https://www.shadertoy.com/view/Md3yRB)  
- [Live Coding:Bending Light - The Art of Code](https://www.youtube.com/watch?v=NCpaaLkmXI8)
- [Shader Coding: KIFS Fractals explained! - The Art of Code](https://youtu.be/il_Qg9AqQkE)
- [Polyhedron again - knighty](https://www.shadertoy.com/view/XlX3zB)
- [Polyhedrons, many many polyhedrons... - Fractalforums](https://www.fractalforums.com/fragmentarium/solids-many-many-solids/30/)
- [Wythoff Mathematical Details - Greg Egan](http://www.gregegan.net/APPLETS/26/WythoffNotes.html)
- [heavy metal squiggle orb - mattz](https://www.shadertoy.com/view/wsGfD3)
- [UN BALLON DE FOOT FRACTAL ... et d’autres objets étranges - Jos Leys](https://images.math.cnrs.fr/Un-ballon-de-foot-fractal.html?lang=es&id_forum=5490)
- [Wythoff Polyhedra with Exact SDF - mla](https://www.shadertoy.com/view/WlccR8)
- [silver tilings - flockaroo](https://www.shadertoy.com/view/XsXfz2)
- [heavy metal squiggle orb  - mattz](https://www.shadertoy.com/view/wsGfD3)
  