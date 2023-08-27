---
layout: default
title : "Symetries and SDFs (Signed Distance Functions)"
tags : shadertoy
---
# Introduction
Symmetry, a ubiquitous feature in the natural world, has always held a profound appeal for the human eye. Our inherent appreciation for balance and harmony draws us to symmetrical forms.

SDFs (Signed Distance Functions) offer a straightforward means of achieving symmetrical patterns, making them a perfect fit for the ray marching algorithm widely employed by creators on platforms like Shadertoy.

Achieving symmetry in an SDF with respect to an axis can be easily accomplished using a simple technique: taking the absolute value of the corresponding coordinate. 

Creating repetition in an SDF can be achieved in a straightforward manner by utilizing the modulo operation on one of the coordinates. 

Implementing polar symmetry in an SDF can be accomplished by utilizing the modulo operation on the angle in polar coordinates. 

By combining the aforementioned techniques, creators can unlock a world of infinite possibilities within SDFs. Applying a combination of taking the absolute value of coordinates, using modulo operations on both Cartesian and polar coordinates, and introducing iterative transformations can lead to the creation of mesmerizing kaleidoscopic patterns. Furthermore, these techniques can also be extended to generate intricate and captivating fractal structures known as KFS fractals. 

Exploring the vast realm of Shadertoy unveils a captivating rabbit hole of topics and possibilities. As creators delve into the depths of this platform, they encounter a plethora of fascinating subjects that can spark further exploration. One such topic is the study of point groups and group theory. Point groups, which describe the symmetry of objects, play a crucial role in understanding and creating symmetrical patterns within shaders. Group theory, a branch of mathematics, provides a formal framework for analyzing and categorizing these symmetries. As creators dive into the intricacies of shaders on Shadertoy, they may find themselves immersed in the exploration of papers and research related to point groups and group theory, deepening their understanding of the underlying principles that govern symmetrical creations. The journey through Shadertoy becomes a gateway to an exciting world where art and mathematics harmoniously converge, inspiring creators to push the boundaries of their imagination and knowledge.

# References

* [Point Group - Wikipedia](https://en.wikipedia.org/wiki/Point_group)
* [Polyhedral group - Wikipedia](https://en.wikipedia.org/wiki/Polyhedral_group)
* [Wikipedia FR](http://wiki.scienceamusante.net/index.php/Groupes_ponctuels_de_sym%C3%A9trie)
* [Wythoff explorer - mattz](https://www.shadertoy.com/view/Md3yRB)

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/7t2cRd?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>

