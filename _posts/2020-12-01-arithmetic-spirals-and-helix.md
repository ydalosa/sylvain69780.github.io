---
layout: default
title : "SDF functions for arithmetic spirals and Helix"
tags : shadertoy
---
# SDF functions for arithmetic spirals and Helix
Helix and spiral shapes are some of the most captivating and mesmerizing designs used in computer graphics. These shapes are known for their elegant and graceful appearance, which can be found in various forms, including DNA structures, galaxies, shells, and many more. The unique patterns and curvatures of helix and spiral shapes make them a popular choice in digital art and design.  

While there are many formulas available for creating helix and spiral shapes using signed distance functions (SDFs), it is important to note that exact SDFs for these shapes do not exist. This is because helix and spiral shapes are mathematically complex and their shapes involve non-linear curves that are difficult to define using simple mathematical functions.

## Helix
[searching using the helix keyword](https://www.shadertoy.com/results?query=helix&sort=popular&from=24&num=12)  

[Helix - distance (APPROXIMATED) - IQ](https://www.shadertoy.com/view/ftyBRd)  
## Arithmetic (Archimedean) spiral


[Basic Archimede Spiral  -  iapafoto](https://www.shadertoy.com/view/NdlyD4)  

[Spiraled Layers - Tater](https://www.shadertoy.com/view/Ns3XWf)

Fabrice's tutorial for loopless spiral [make a spiral](https://shadertoyunofficial.wordpress.com/2019/01/15/case-study-making-dot-pattern-loopless/)  

Usefull formulas [Walking spirals - mrange](https://www.shadertoy.com/view/ddcGDl)  
```cpp
float spiralLength(float b, float a) {
  // https://en.wikipedia.org/wiki/Archimedean_spiral
  return 0.5*b*(a*sqrt(1.0+a*a)+log(a+sqrt(1.0+a*a)));
}

void spiralMod(inout vec2 p, float a) {
  vec2 op     = p;
  float b     = a/TAU;
  float  rr   = length(op);
  float  aa   = atan(op.y, op.x);
  rr         -= aa*b;
  float nn    = mod1(rr, a);
  float sa    = aa + TAU*nn;
  float sl    = spiralLength(b, sa);
  p           = vec2(sl, rr);
}
```


sdSpiral function used for the rolled bed [On the salt lake - iapafoto](https://www.shadertoy.com/view/fsXcR8)  

```cpp
float sdSpiral(vec3 p, vec2 sz){
    float d = length(p.xy)/sz.x + atan(-p.y,p.x)*.5;  
    d -= PI*clamp(round(d/PI),1.,4.);
    return max(sz.x*(abs(d)-.77), abs(p.z)-sz.y); 
}
```

[Spiral Arcs - distance - IQ](https://www.shadertoy.com/view/sssyWN)

## Other ideas

[Développante du cercle](https://fr.wikipedia.org/wiki/D%C3%A9veloppante_du_cercle)

## Generic Spriral ribbon SDF

In the domain of computer graphics, having an SDF function for an arithmetic spiral would be extremely useful for artists and designers who create digital art and 3D models. 

Having an SDF function for an arithmetic spiral that takes as input a start position, length, width, and space between spirals would be useful in a variety of applications.
