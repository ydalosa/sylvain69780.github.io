---
layout: default
title: "Puzzling Rhombic Dodecahedron"
tags: shadertoy
---

# Puzzling Rhombic Dodecahedron
## What is this ?

I have the chance to own a "scorpius slider spider puzzle", a piece of wood art offered to me more than 30 years ago as I was a child.

![preview](https://static.blog4ever.com/2008/06/213622/artfichier_213622_8769121_202010012502419.png)

I learnt reading [Philippe Cichon's blog](https://puzzles-et-casse-tete.blog4ever.com/le-scorpius-1) that it's a rare puzzle, very few makers exists.

This video of Philippe Cichon shows how to mount and unmout it (in French)  
[Le Scorpius on Youtube](https://www.youtube.com/watch?time_continue=13&v=2orJ6rTSx2s&feature=emb_logo)

## Calculation of the angle between each piece

I was very intrigued by the angle needed to fit four pieces together.  
Refering to the article on [Philippe Cichon's blog (French)](https://puzzles-et-casse-tete.blog4ever.com/le-scorpius-1), this angle is 70 degres, but why this value ?  

The explanation is in the design of this puzzle, mentioned in the Stewart Coffin's book **The Puzzling World of Polyhedral Dissections**.

![preview](https://johnrausch.com/PuzzlingWorld/images/fig093.gif)  

This [sentence](https://johnrausch.com/PuzzlingWorld/chap08.htm) of the book explains all the mystery.  
>The rhombic dodecahedron can be totally enclosed by a symmetrical cluster of 12 sticks having equilateral-triangular cross-section.  

The Wikipedia page about the [Rhombic dodecahedron](https://en.wikipedia.org/wiki/Rhombic_dodecahedron) mention that $$\arccos{\frac{1}{3}}$$ is the acute angles on each face. 

|symbol|value|explanation|calculation|
|---|---|---|---|
|rdan|70,5°| the acute angles on each face of the rhombic dodecahedron|acos(1/3)|
  
The French version of Wikipedia give a different expression for it : **2*arctan(1/√2)** ,it appears that this is the same value !

This is related to the tan of the half arc formula.  

[Fonction circulaire réciproque - Wikipedia](https://fr.wikipedia.org/wiki/Fonction_circulaire_r%C3%A9ciproque)  
[Tangent half-angle formula - Wikipedia](https://en.wikipedia.org/wiki/Tangent_half-angle_formula)  

```
acos(x) = 2*atan(sqrt(1-x*x)/(1+x))  
// Applied with x = 1/3
acos(1/3) = 2*atan(sqrt(8/9)/(4/3)  
= 2*atan( (sqrt(2)*2/3)/(4/3) ))   
= 2*atan(1/sqrt(2))   
```

## Optimisation and trigonometry functions

These articles of IQ about using vectors operations (dot and cross products) instead of trigonometry function interested me.  
In order to convince developers that are not sensible to performance, it is said that some Kitten may be killed.

This one to make you know the relation between cos and dot product and sin and cross product.

[Inigo Quilez   ::   articles  ::   avoiding trigonometry - 2013](https://iquilezles.org/www/articles/noacos/noacos.htm)  

This one to make you understand the formulas   

```
sin(α + β)=sin(α) * cos(β) + cos(α) * sin(β)
cos(α + β)=cos(α) * cos(β) - sin(α) * sin(β)
```

[Inigo Quilez   ::   articles  ::   a sin/cos trick - 2010](https://iquilezles.org/www/articles/sincos/sincos.htm)  

## Position the pieces on the Rhombic dodecahedron

![preview](https://sylvain69780.github.io/assets/images/scorpius_puzzle_face1.svg)  

Details of the front view of one of the 24 puzzle's pieces.
The diameters of the holes and the pivots can be 6,0 mm, drilling no more than 8,0 mm. Seems to me 5,0 mm is good enough.  

|symbol|value|explanation|calculation|
|---|---|---|---|
|trnc|10%|pencentage of trucation of the top of the piece.|free choosen value, adds complexity in the calculations compared to a simple triangular shape. My own version has no truncation.|
|k|1,732|square root of 3, Pythagorean theorem applied to the height of an half equilateral triangle.|sqrt(3)|
|f1|15.0 mm|width of face 1|unit for calculations|
|f2|27.0 mm|width of face 2| 2 * f1 * (1-trnc)|
|f3|1.5 mm|width of face 3| f1 * trnc|
|f4|23.4 mm|width of face 3| f1 * k * (1-trnc) |

![preview](https://sylvain69780.github.io/assets/images/scorpius_puzzle_rhombus.svg)  

Top view of face 1 on the Rhombic dodecahedron rhombus face and developed view of face 2. Face 1 is where most of the magic happens, because it's the contact face this the inner Rhombic Dodecahedron. This is also the location of the pivots that must match an hole on face 2 of the locked neighbouring piece.  

![preview](https://sylvain69780.github.io/assets/images/scorpius_puzzle_developped.svg)  

Developped view  

|symbol|value|explanation|calculation|
|---|---|---|---|
|rdan|70,5°| the acute angles on each face of the rhombic dodecahedron|acos(1/3)|
|d1|15,91 mm|half side of the Rhombic face.|f1/sin(rdan)|
|d2|14,32 mm|Corrected (because of truncation) half side of the Rhombic face.|d1 * (1-trnc) *or* (1-trnc) * f1/sin(rdan)|
|slope|35,36%|slope of the bended lines in comparison with an horizontal line. Usefull to compute the point coordonates.|1/tan(rda) *or* 1/(3 * sin(rda))|
|A.x|15,00 mm|x Coordinate of A from the rhombus center. This point can be visualized as the bottom point of the 6 "craters" of the puzzle ! |f1|
|A.y|21,21 mm|y Coordinate of A from the rhombus center.|d1+f1 * slope *or* f1 * (4/3)/sin(rdan)|
|B.x|-7,50 mm|x Coordinate (relative to A) of B, pivot point on face 1.|-f1/2|
|B.y|11,67 mm|y Coordinate (relative to A) of B, pivot point on face 1.|d2-f1 * slope/2|
|C.x|13,5 mm|x Coordinate (relative to A) of C, pivot point on face 2.|f2/2|
|C.y|-3,18 mm|y Coordinate (relative to A) of C, pivot point on face 2.|(f2 * slope-d1)/2|

Details of the calculations using Desmos.  
 
<iframe src="https://www.desmos.com/calculator/jjntvihj13?embed" width="500" height="500" style="border: 1px solid #ccc" frameborder=0></iframe>  

![preview](https://sylvain69780.github.io/assets/images/scorpius_puzzle.svg)  

Verification of the calculations using Shadertoy.  
Seems it works !  

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/Nlf3W2?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>  

## Is it possible to use polar domain repetition ?

Using the Ray marching algorithm, domain repetition enables to compute a shape only once by defining repetition domains, like a grid for example.   
Because in the animation just shown before, this is not applicable easyly, and the rendering is very slow because each of the 24 pieces are evaluated for distance estimation.

## About The Puzzling World of Polyhedral Dissections

With Philippe Cichon's blog, I discovered a puzzling book available online for free !

The Stewart Coffin's book **The Puzzling World of Polyhedral Dissections** has been made available on the Internet by [John Rausch](https://www.puzzle-place.com/wiki/John_Rausch)

>The Puzzling World of Polyhedral Dissections was obviously not written by Stewart to get rich. Anyone who produces such works does it as a labor of love for a subject that is very dear to them. As puzzle collectors, we owe Stewart a huge debt of gratitude for sharing with us his knowledge about the mathematics, aesthetics and philosophy of geometric puzzles. If you enjoy it as much as I do, drop Stewart a line and thank him for his unselfish gesture.

>So, during a recent visit with Stewart, I asked him what he thought about publishing it on the Internet. I hope you are as happy as I am that he said, "go for it!"

>John Rausch
>Oregonia, Ohio 1998

## References

- [PuzzlingWorld By Stewart T. Coffin](https://johnrausch.com/PuzzlingWorld/contents.htm)  

- Rhombic dodecahedron SDF from yx on Shadertoy

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/Wd2Gzt?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>  

- My illustrations on Shadertoy

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/slfSRj?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>  

<iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/slSGzy?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>  

- [Adam Savage's One Day Builds: Rhombic Dodecahedron with Matt Parker!](https://www.youtube.com/watch?v=65r_1TzJXaQ)  

- My illustration using [p5js](https://editor.p5js.org/sylvain69780/full/rCCoddv44) and loading an OBJ mesh file created using Autodesk fusion 360 (free for personal use).  

Reference : [The Coding Train : 18.7: Loading OBJ Model - WebGL and p5.js Tutorial](https://youtu.be/FUI7HEEz9B0)  

<iframe  width="640" height="360" frameborder="0" src="https://preview.p5js.org/sylvain69780/embed/rCCoddv44"></iframe>  
