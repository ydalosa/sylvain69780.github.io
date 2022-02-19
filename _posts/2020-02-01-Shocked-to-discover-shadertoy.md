---
layout: default
title:  "Down the Rabbit hole : Shadertoy"
tags : shadertoy
---

In March 2020, the COVID made me having more time to search about creative coding.

And in fact I discovered more than I expected !

>In English, we use the expression down the rabbit hole when we get so caught up in the search for something – like an answer to a problem – that we end up somewhere totally different.

{% for shader in site.data.shaders reversed %}
{% if shader.id == "3lsSzf" %}
![{{ shader.title }}](https://www.shadertoy.com/media/shaders/{{ shader.id }}.jpg)  
**[{{ shader.title }}](https://www.shadertoy.com/view/{{ shader.id }})** by **[{{ shader.author }}](https://www.shadertoy.com/user/{{ shader.author }})**

>{{ shader.comments }} 
{% endif %}
{% endfor %}

* I (re)discovered the **demoscene** and their community very active on Shadertoy.
* I **discovered** with Shadertoy **the ray marching** algorithm that allows to build real time realistic 3D scenes "without any Maya or Blender or something", just your keyboard, some calculations on a paper sheet and knownlege of how to make a "for" loop.  

Shadertoy has been designed from the origin as a social media that enables **graphics enthusiasts** to create and share computer graphics knowledge.  

[Pol Jeremias Vila](http://www.poljeremias.com/#about)

