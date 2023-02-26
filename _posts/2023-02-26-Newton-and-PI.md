---
layout: default
title: "The Discovery That Transformed Pi"
tags: coding
---
# The Discovery That Transformed Pi

I find the Veritasium video "The Discovery That Transformed Pi" quite extraordinary.  

It shows Newton as a real genius, in the sense that he invented a revolutionary method of calculation starting from a trivial figure (Pascal's triangle) known for centuries.

The generalized binomial expansion is below. 

$$
(1+x)^n = 1 + nx + \frac{n\times(n-1)}{2!}x^2 + \frac{n\times(n-1)\times(n-2)}{3!}x^3 + ...
$$ 

Taking $n = \frac{1}{2}$, a fraction, for a formula supposed to be used with integer values (!), magic happens.

I wanted to compute PI with the formula described in the Video.

$$
\pi = \frac{3\times\sqrt{3}}{4} + \sum_{n=0}^{\infty}\frac{(2n)!}{2^{4n+2}\times(n!)^2\times(2n-1)(2n+3)}
$$ 

I wanted also compute the square root of 3 as Newton would do, using the following formula. $(x+1)^\frac{1}{2}$ with $x=-\frac{1}{4}$  

$$ 
sqrt{3} = 2\times(1-\frac{1}{4})^\frac{1}{2} 
$$ 

```python
from math import *
def fact(n):
    f=1
    for k in range(n):
        f=f*(k+1)
    return f
def factk(n,c):
    f=1
    for k in range(c):
        f=f*(n-k)
    return f
def newton_sqr3(c):
    n=1/2
    x=-1/4
    s=1
    for k in range(1,c):
        s+=(x**k)*factk(n,k)/fact(k)        
    return s*2 
def newton_pi(c):
    pi=(3*newton_sqr3(c)/4)
    for k in range(0,c):
        pi-=24*fact(2*k)/((2**(4*k+2))*(fact(k)**2)*(2*k-1)*(2*k+3))
    return pi
for i in range(1,21):
    print(str(i)+" iterations.")
    print(newton_pi(i))
    print(pi)
    print()
```

This gives after 20 iterations, quite same number as the pi of the math library, that is geat !

```
20 iterations.
3.1415926535897993
3.141592653589793
```


# References

[The Discovery That Transformed Pi - Veritasium](https://www.youtube.com/watch?v=gMlf1ELvRzc)  
[Calculating π by hand the Isaac Newton way: Pi Day 2020](https://www.youtube.com/watch?v=CKl1B8y4qXw)  
[NEWTON’S APPROXIMATION FOR PI](https://think-maths.co.uk/sites/default/files/2020-03/Newton%27s%20Approximation%20to%20Pi_1.pdf)  
[TEACHER NOTES AND SOLUTIONS](https://think-maths.co.uk/sites/default/files/2020-03/Teacher%20Notes%20and%20Solutions_5.pdf)  
