---
layout: default
title: "Isaac Newton and the decimals of PI"
tags: coding
---
# Isaac Newton and the decimals of PI

I find this Veritasium video quite extraordinary. I love this way of telling a story of people who changed the world forever by their discoveries.

$$
\pi = \frac{3\times\sqrt{3}}{4} + \sum_{n=0}^{\infty}\frac{(2n)!}{2^{4n+2}\times(n!)^2\times(2n-1)(2n+3)}
$$ 
To calculate the square root of 3, Newton was able to use the following formula.
Generalized binomial expansion of 
$$ 
(4-1)^\frac{1}{2}
$$ 

```python
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

def newton_pi(c):
    n=1/2
    x=-1/4
    s=1
    for k in range(1,c):
        s+=(x**k)*factk(n,k)/fact(k)        
    sqr3=s*2
    pi=(3*sqr3/4)
    for k in range(0,c):
        pi-=24*fact(2*k)/((2**(4*k+2))*(fact(k)**2)*(2*k-1)*(2*k+3))
    return pi

for i in range(1,20):
    print(newton_pi(i))
```

This gives 
```
3.5
3.1625
3.1440848214285717
3.141968936011905
3.1416568244174448
3.1416044456896053
3.141594932788299
3.1415931104639387
3.14159274772605
3.1415926734073403
3.141592657834339
3.141592654511837
3.141592653792465
3.141592653634789
3.1415926535998686
3.141592653592066
3.141592653590309
3.1415926535899112
3.14159265358982
```


# References

[The Discovery That Transformed Pi - Veritasium](https://www.youtube.com/watch?v=gMlf1ELvRzc)
[Calculating π by hand the Isaac Newton way: Pi Day 2020](https://www.youtube.com/watch?v=CKl1B8y4qXw)
[NEWTON’S APPROXIMATION FOR PI](https://think-maths.co.uk/sites/default/files/2020-03/Newton%27s%20Approximation%20to%20Pi_1.pdf)
[TEACHER NOTES AND SOLUTIONS](https://think-maths.co.uk/sites/default/files/2020-03/Teacher%20Notes%20and%20Solutions_5.pdf)
