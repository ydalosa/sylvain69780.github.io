---
layout: default
title: "Impress your friends by Live coding the manderbrot's Set fractal on Shadertoy"
tags: shadertoy
---

I know because I already coded it on my ATARI 520 ST, it's a very easy fractal to compute.

The definition is trivial, if you just have an idea of what complex numbers are (it's not so complex !)

You just need this little complex number multiplication function to compute Z*Z.

You can cut and paste the code below in Shadertoy as an example

```
#define MAX_STEP 256
#define cmpxmul(a, b) vec2(a.x*b.x-a.y*b.y, a.x*b.y+a.y*b.x)
void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    // Normalized pixel coordinates (from 0 to 1)
    vec2 uv = (2.0*fragCoord-iResolution.xy)/iResolution.y;
    uv *= 2.;
    // Time varying pixel color
    vec3 col = vec3(0);
    
    vec2 m = (2.0*iMouse.xy-iResolution.xy)/iResolution.y;
    if ( iMouse.x > 0.0 ) {
        uv *= 1.0 + m.y;
        uv.x -= m.x;
    }
    
    vec2 z = vec2(0);
    
    int i;
    for ( i=0 ; i<MAX_STEP ; i++ ) {
        z = cmpxmul(z,z) + uv;
        if ( dot(z,z) > 100. ) break;
    }
    
    // colors
    if ( i < MAX_STEP ) {
        col = vec3(float(i+1)/float(MAX_STEP));
    }

    // Output to screen
    fragColor = vec4(col,1.0);
}
```

Iq made a short version of it here :

<iframe width="320" height="180" frameborder="0" src="https://www.shadertoy.com/embed/lllGWH?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>


If you want to know about this fractal, you are in the good place on Shadertoy. I'am impressed by the articles IQ wrote on this topic.

Please note that there is an infinite zoom possible on the Mandelbrot's set, the only limits are the precision of the float and the number of allowed iterations to compute the details of the set.

References 

- [The definition of Manderbrot set - Wikipedia](https://en.wikipedia.org/wiki/Mandelbrot_set)
- [Inigo Quilez   ::   articles  ::   smooth iteration count for generalized Mandelbrot sets - 2007](https://www.iquilezles.org/www/articles/mset_smooth/mset_smooth.htm)
- [Inigo Quilez   ::   articles  ::   the main bulb of the Mandelbrot set - 2006](https://www.iquilezles.org/www/articles/mset_1bulb/mset1bulb.htm)
- [Inigo Quilez   ::   articles  ::   the period-2 bulb of the Mandelbrot set - 2006](https://www.iquilezles.org/www/articles/mset_2bulb/mset2bulb.htm)
- [Inigo Quilez   ::   articles  ::   area of the main bulb of the Mandelbrot set - 2006](https://www.iquilezles.org/www/articles/mset_area/msetarea.htm)
- [Inigo Quilez   ::   articles  ::   the symmetry of the Mandelbrot set - 2006](https://www.iquilezles.org/www/articles/mset_symmetry/msetsymmetry.htm)
- [Inigo Quilez   ::   articles  ::   introduction to the mandelbrot set - 2001](https://www.iquilezles.org/www/articles/arquimedes/arquimedes.htm)
