---
layout: default
title: "The Discovery That Transformed Pi"
tags: coding
---
# PI day

Pi Day is **March 14**. This is an opportunity to test all sorts of ideas for calculating this number.  
I loved David Shiftman's song on the youtube channel "The coding train".  
This is the only number you can eat ! 🥮

[Coding Challenge 169: Pi in the Sky Game](https://www.youtube.com/watch?v=_H9JIwWP7HQ)
# The Discovery That Transformed Pi

The Veritasium video "The discovery that transformed Pi" is extraordinary.

This video has **11M views**, and it's no coincidence.

It shows Newton as a real genius, in the sense that he invented a revolutionary method of calculation starting from the trivial "binomial expansion", related to Pascal's triangle and known for centuries.

The binomial expansion is below. 

$$
(1+x)^n = 1 + nx + \frac{n\times(n-1)}{2!}x^2 + \frac{n\times(n-1)\times(n-2)}{3!}x^3 + \frac{n\times(n-1)\times(n-2)\times(n-3)}{4!}x^4 + ...
$$ 

Taking n as an integer, the serie is not infinite because a zero appears when k=n and all remaining terms are multiplied by zero. 
Taking $n = \frac{1}{2}$, a fraction, for a formula designed to be used with integer values, magic happens.

I wanted to compute PI with the formula described in the Video.

$$
\pi = \frac{3\times\sqrt{3}}{4} - 24\times\sum_{n=0}^{\infty}\frac{(2n)!}{2^{4n+2}\times(n!)^2\times(2n-1)(2n+3)}
$$ 

I wanted also compute the square root of 3 as [Newton would do](https://youtu.be/gMlf1ELvRzc?t=742), using the following formula. $(x+1)^\frac{1}{2}$ with $x=-\frac{1}{4}$  

$$ 
sqrt{3} = 2\times(1-\frac{1}{4})^\frac{1}{2} 
$$ 

$$ 
(1-\frac{1}{4})^\frac{1}{2} = 1 - \frac{1}{8} - \frac{1}{128} - \frac{1}{2^{10}} + \frac{5}{2^{15}} ...
$$ 

The code below is not optimized at all.

```python
from math import *
# factorial function
def fact(n):
    f=1
    for k in range(n):
        f=f*(k+1)
    return f
def newton_sqr3(c):
    n=1/2
    x=-1/4
    s=1
    # computes n*(n-1)*(n-2)... for sqrt(3), but here n will be 1/2
    num=n
    for k in range(1,c):
        s+=(num/fact(k))*(x**k)
        num*=(n-k)
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

This gives after 20 iterations, a number very close to the pi of the math library, that is great for a first try !

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
