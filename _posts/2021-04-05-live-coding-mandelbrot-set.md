---
layout: default
title: "Impress your friends by Live coding the manderbrot's Set fractal on Shadertoy"
tags: shadertoy
---

I know because I already coded it on my ATARI 520 ST, it's a very easy fractal to compute.

The definition is trivial, if you just have an idea of what complex numbers are (it's not so complex !)

You just need this little complex number multiplication function to compute Z*Z.

```
vec2 cmpxmul(in vec2 a, in vec2 b) {
	return vec2(a.x * b.x - a.y * b.y, a.y * b.x + a.x * b.y);
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
