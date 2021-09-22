---
layout: default
title: "Notes about ligthing"
tags: shadertoy
---
# Some notes for nice ligthing effects

Again, I'am not a professional graphic programmer, the goal here is to have fun !

Ray marching is fun, and allows to use the signed distance function of the scene for many nice lighing effects. 

The lighting topic is huge, hopefully this will help you to find some usefull information in the links I profide here.

Don't take it too seriously, just pick up some nice ideas for your next shader !

Related references :
- [Outdoors lighting - Inigo Quilez](https://www.iquilezles.org/www/articles/outdoorslighting/outdoorslighting.htm)

# Ambiant lighing

It's just a color the material has assigned when there is no lighting at all.
Very flat and boring, may be needed in some situations to avoid totally black zones.

# Diffuse ligthing

This is the simpliest of all, there is an use of the dot product between the surface normal and the light direction to get an intensity.

Here the artistic work starts because we can paint the material with different colors coming from light sources.

- We can choose to have 3 lights sources to simulate an outdoor scene, with the sun as keylight, the sky as second light, and indirect light supposed to come from the reflexion of the sun light on the environment. Colors can be choosen accordingly. ( yellow, blue, green for example ) 
- For an indoor scene, I saw Shane's examples of just a point light, positioned not far from the camera position.

Related references :
- [Outdoors lighting - Inigo Quilez](https://www.iquilezles.org/www/articles/outdoorslighting/outdoorslighting.htm)

# Occlusion

This effect creates a very subtile shadow.
It helps the viewer to understand the concavities of the object we are looking at.
Procedural occulsion calculations are very frequent on Shadertoy, and this add a lot of details.

Related references :
- [Outdoors lighting - Inigo Quilez](https://www.iquilezles.org/www/articles/outdoorslighting/outdoorslighting.htm)

# Fake IBL

This is a fast way I found to create a metal like effect.

<iframe width="320" height="180" frameborder="0" src="https://www.shadertoy.com/embed/tlscDB?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>

Related references :
- [blackle's fake ibl](https://www.twitch.tv/videos/590616102)

# IQ magic specular ligthing

This sun_spe term below make the magic of the rendering.

Question : how does it works ? 

>I did my own interpretation of specularity !

Specularity is about reflexion of the light on a surface, as "specular" means mirror.

```
    vec3  sun_hal = normalize( sun_lig-rd );
    float sun_spe = ks*pow(clamp(dot(nor,sun_hal),0.0,1.0),8.0)*sun_dif*(0.04+0.96*pow(clamp(1.0+dot(sun_hal,rd),0.0,1.0),5.0));

    col += sun_spe*vec3(8.10,6.00,4.20)*sun_sha;

```

Just try to comment it in my Rocket Toy shader to see the difference.  

<iframe width="320" height="180" frameborder="0" src="https://www.shadertoy.com/embed/3dSBRG?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>

I very lately realized that this is a variation of the Blinn-Phong reflection model, with a bit of fresnel, and "hal" means "halfway vector". 

Related references :
- [IQ Video sequence about it is very short : 4:09](https://youtu.be/Cfe5UQ-1L9Q?t=14952)
- [Why aren't Mirrors White? Why isn't EVERYTHING a Mirror? - The Science Asylum](https://youtu.be/1n_otIs6z6E)
- [Blinn–Phong reflection model - Wikipedia](https://en.wikipedia.org/wiki/Blinn%E2%80%93Phong_reflection_model)

# Other sources

[Raymarch template GGX - darkeclipz](https://www.shadertoy.com/view/3tyyWm)
[Specular highlight models  - darkeclipz](https://www.shadertoy.com/view/WtycWW)
[Physically Based Shading at Disney](https://neil3d.github.io/assets/pdf/s2012_pbs_disney_brdf_notes_v3.pdf)