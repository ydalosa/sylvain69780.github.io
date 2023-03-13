---
layout: default
title : "SDF functions for arithmetic spirals and Helix"
tags : shadertoy
---
# SDF functions for arithmetic spirals and Helix
## Intro
Helix and spiral shapes are some of the most captivating and mesmerizing designs used in computer graphics. These shapes are known for their elegant and graceful appearance, which can be found in various forms, including DNA structures, galaxies, shells, and many more. The unique patterns and curvatures of helix and spiral shapes make them a popular choice in digital art and design.  

While there are many formulas available for creating helix and spiral shapes using signed distance functions (SDFs), it is important to note that exact SDFs for these shapes do not exist. This is because helix and spiral shapes are mathematically complex and their shapes involve non-linear curves that are difficult to define using simple mathematical functions.
In the domain of computer graphics, having an SDF function for an arithmetic spiral would be extremely useful for artists and designers who create digital art and 3D models. 

Having an SDF function for an arithmetic spiral that takes as input a start position, length, width, and space between spirals would be useful in a variety of applications.  

In fact I was also eager to learn and spent an unreasonably long time on this shader calculating an approximate distance to get there.
## Create a generic "Ribbon spiral" SDF in Shadertoy
### Before we start

I highly recommend watching this video first if you don't know what polar coordinates are. Then let's go !

<iframe width="560" height="315" src="https://www.youtube.com/embed/r1UOB8NVE8I" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

In polar coordinates we need to compute an angle and a distance to the center for each point on the screen.

### Center distance 
Let's start with the distance to the center. The code below allows to visualize this distance.

The **floor** function is used to create some colored regions, and the **fract** fuction to give the distance of the point to the center of each region.  Instead of fract that gives a result between 0 and 1 , you can use r-round(r) that directly gives a value between -0.5 and 0.5.

This is called "domain repetition" and is widely used on Shadertoy.

```cpp
void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    vec2 uv = (fragCoord-.5*iResolution.xy)/iResolution.y;
    uv *=10.;
    float e = 20./iResolution.y;
    vec3 col = vec3(0);
    float r = length(uv);
    float id = floor(r), rg = fract(r)-.5;
    col += id*.1 * (id > 0.0 ? vec3(1.000,0.522,0.522) : -vec3(0.478,0.478,1.000));
    col += smoothstep(e,-e,abs(rg));
    fragColor = vec4(sqrt(col),1.0);
}
```

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/csK3Ww?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>

### Angle

The idea now is to shift these concentric circles so that they form a spiral. For this we will use the second component of the polar coordinates, the angle with the X axis. This is obtained with the atan function.

I noticed that in the video that Martin used **atan(x,y)**. The typical usage is instead **atan(y,x)** which then gives the angle between the X axis and segment from the origin to the point p on the screen. 
the code below allows to visualize the output value of the function atan(y,x) for all the points on the screen. These values range from -PI to +PI.

```cpp
void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    vec2 uv = (fragCoord-.5*iResolution.xy)/iResolution.y;
    vec3 col = vec3(0);
    float a = atan(uv.y,uv.x)/6.28;
    a = floor(a*16.)/16.;
    col += a * (a > 0.0 ? vec3(1.000,0.522,0.522) : -vec3(0.478,0.478,1.000));
    fragColor = vec4(sqrt(col),1.0);
}
```

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/stdcDS?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>

### Draw a spiral

The definition found on wikipedia for the arithmetic spiral is $r = b \times a$. 

On first lesson is to use the same names for the variables in the code to avoid any confusion.

As I want the spiral size to increases by 1 units every turn but will just put $b = \frac{1}{2\times\pi}$

When we add b times the angle given by atan function to the distance to the center we get a new distance and here is the spiral !

```cpp
#define TAU 6.28318530718
void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    vec2 uv = (fragCoord-.5*iResolution.xy)/iResolution.y;
    uv *=10.;
    float e = 20./iResolution.y;
    vec3 col = vec3(0);
    float r = length(uv);
    float b=1./TAU;
    float sector = atan(uv.y,uv.x);
    float grad = r+sector*b;
    float id = floor(grad), rg = fract(grad)-.5;
    col += id*.1 * (id > 0.0 ? vec3(1.000,0.522,0.522) : -vec3(0.478,0.478,1.000));
    col += smoothstep(e,-e,abs(rg));
    fragColor = vec4(sqrt(col),1.0);
}
```

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/cdKGDw?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>

### Draw a portion of the spiral

In order to draw only a portion of the spiral, we can limit the domain repetition.

This is ordinarily done using a clamp glsl function like this.

```cpp
float id = floor(grad), rg = (grad-clamp(id,2.,4.))-.5;
```

But what if we want a portion of the spiral between 2 random **a** values **a1** and **a2** ?

We can clamp using the more complex code below.

```cpp
float id = floor(grad), rg = grad-round(min(max(r,b*a1+.5),b*a2-.5)+b*sector);
```
<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/cdVGDw?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>

I put 2 red circles and a green coloring to show how the limits of the domain repetition as set.

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/csV3Wm?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>

### To be continued

* Compute UV coordinates

We can identify a point in "Spiral coordinates" (u,v), using u as length of the spiral, and v as how far away the point is from the line of the spiral. We can use.

$$\large u=\frac{b}{2}\times(a\times\sqrt{1+a^2}+\log(a+\sqrt{1+a^2}))$$

* Get the a value for a given length of the spiral.

We can use Newton Raphson method taking advantage of the fact that the derivative is easy and starting with $a=\sqrt{u}$

$$\large u\prime=b\times\sqrt{1+a^2}$$

* Finished shader below [SDF for Archimedean Spiral](https://www.shadertoy.com/view/stB3WK)

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/stB3WK?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>

## References
[Formulas on Wikipedia](https://en.wikipedia.org/wiki/Archimedean_spiral#:~:text=The%20Archimedean%20spiral%20has%20the,the%20name%20%22arithmetic%20spiral%22.)

[searching using the helix keyword](https://www.shadertoy.com/results?query=helix&sort=popular&from=24&num=12)

[Helix - distance (APPROXIMATED) - IQ](https://www.shadertoy.com/view/ftyBRd)

[On the salt lake - iapafoto](https://www.shadertoy.com/view/fsXcR8)  Have a look at the sdSpiral function used for the rolled bed.

[Basic Archimede Spiral  -  iapafoto](https://www.shadertoy.com/view/NdlyD4)

[Spiraled Layers - Tater](https://www.shadertoy.com/view/Ns3XWf)

[Spiral Arcs - distance - IQ](https://www.shadertoy.com/view/sssyWN)

[Développante du cercle](https://fr.wikipedia.org/wiki/D%C3%A9veloppante_du_cercle)

[Walking spirals - mrange](https://www.shadertoy.com/view/ddcGDl)

Fabrice's tutorial for loopless spiral [make a spiral](https://shadertoyunofficial.wordpress.com/2019/01/15/case-study-making-dot-pattern-loopless/)  

