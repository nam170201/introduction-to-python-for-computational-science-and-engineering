---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.12
    jupytext_version: 1.7.1
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Numerical Computation

## Numbers and numbers

We have already seen (03 Data Types Structures, Numbers) that Python knows different *types* of numbers:

-   `float`ing point numbers such as 3.14

-   `int`egers such as 42

-   `complex` numbers such as 3.14 + 1*j*

### Limitations of number types

#### Limitations of `int`s

Mathematics provides the infinite set of natural numbers ℕ = {1, 2, 3, …}. Because the computer has *finite* size, it is impossible to represent all of these numbers in the computer. Instead, only a small subset of numbers is represented.

The `int`-type can (usually[3]) represent numbers between -2147483648 and +2147483647 and corresponds to 4 bytes (that’s 4\*8 bit, and 2<sup>32</sup> = 4294967296 which is the range from -2147483648 and +2147483647).

You can imagine that the hardware uses a table like this to encode integers using bits (suppose for simplicity we use only 8 bits for this):

|  natural number|  bit-representation|
|---------------:|-------------------:|
|               0|            00000000|
|               1|            00000001|
|               2|            00000010|
|               3|            00000011|
|               4|            00000100|
|               5|            00000101|
|               ⋮|                   ⋮|
|             254|            11111110|
|             255|            11111111|

Using 8 bit we can represent 256 natural numbers (for example from 0 to 255) because we have 2<sup>8</sup> = 256 different ways of combining eight 0s and 1s.

We could also use a slightly different table to describe 256 integer numbers ranging, for example, from -127 to +128.

This is *in principle* how integers are represented in the computer. Depending on the number of bytes used, only integer numbers between a minimum and a maximum value can be represented. On today’s hardware, it is common to use 4 or 8 bytes to represent one integer, which leads exactly to the minimum and maximum values of -2147483648 and +2147483647 as shown above for 4 bytes, and +9223372036854775807 as the maximum integer for 8 bytes (that’s ≈9.2 ⋅ 10<sup>18</sup>).

#### Limitations of `float`s

The floating point numbers in a computer are not the same as the mathematical floating point numbers. (This is exactly the same as the (mathematical) integer numbers not being the same as the integer numbers in a computer: only a *subset* of the infinite set of integer numbers can be represented by the `int` data type as shown in [Numbers and numbers](#Numbers-and-numbers)). So how are floating point numbers represented in the computer?

-   Any real number *x* can be written as
    *x* = *a* ⋅ 10<sup>*b*</sup>
     where *a* is the mantissa and *b* the exponent.

-   Examples:

| x                                 | a       | b  |
|-----------------------------------|---------|----|
| 123.45 = 1.23456 ⋅ 10<sup>2</sup> | 1.23456 |  2 |
| 1000000 = 1.0 ⋅ 10<sup>6</sup>    | 1.00000 |  6 |
| 0.0000024 = 2.4 ⋅ 10<sup>-6</sup> | 2.40000 | -6 |

+++

-   Therefore, we can use 2 integers to encode one floating point number!

    *x* = *a* ⋅ 10<sup>*b*</sup>

+++

-   Following (roughly) the IEEE-754 standard, one uses 8 bytes for one float *x*: these 64 bits are split as

    -   10 bit for the exponent *b* and

    -   54 bit for the mantissa *a*.

This results in

-   largest possible float ≈10<sup>308</sup> (quality measure for *b*)

-   smallest possible (positive) float ≈10<sup>−308</sup> (quality measure for *b*)

-   distance between 1.0 and next larger number ≈10<sup>−16</sup> (quality measure for *a*)

+++

Note that this is *in principle* how floating point numbers are stored (it is actually a bit more complicated).

#### Limitations of `complex` numbers

The `complex` number type has essentially the same limitations as the `float` data type (see [limitations of floats](#Limitations-of-floats)) because a complex number consists of two `floats`: one represents the real part, the other one the imaginary part.

#### …are these number types of practical value?

In practice, we do not usually find numbers in our daily life that exceed 10<sup>300</sup> (this is a number with 300 zeros!), and therefore the floating point numbers cover the range of numbers we usually need.

However, be warned that in scientific computation small and large numbers are used which may (often in intermediate results) exceed the range of floating point numbers.

-   Imagine for example, that we have to take the fourth power of the constant ℏ = 1.0545716 ⋅ 10<sup>−34</sup>*k**g**m*<sup>2</sup>/*s*:

-   ℏ<sup>4</sup> = 1.2368136958909421 ⋅ 10<sup>−136</sup>*k**g*<sup>4</sup>*m*<sup>8</sup>/*s*<sup>4</sup> which is “halfway” to our representable smallest positive float of the order of 10<sup>−308</sup>.

If there is any danger that we might exceed the range of the floating point numbers, we have to *rescale* our equations so that (ideally) all numbers are of order unity. Rescaling our equations so that all relevant numbers are approximately 1 is also useful in debugging our code: if numbers much greater or smaller than 1 appear, this may be an indication of an error.

### Using floating point numbers (carelessly)

We know already that we need to take care that our floating point values do not exceed the range of floating point numbers that can be represented in the computer.

There is another complication due to the way floating point numbers have to be represented internally: not all floating point numbers can be represented exactly in the computer. The number 1.0 can be represented exactly but the numbers 0.1, 0.2 and 0.3 cannot:

```{code-cell} ipython3
'%.20f' % 1.0
```

```{code-cell} ipython3
'%.20f' % 0.1
```

```{code-cell} ipython3
'%.20f' % 0.2
```

```{code-cell} ipython3
'%.20f' % 0.3
```

Instead, the floating point number “nearest” to the real number is chosen.

This can cause problems. Suppose we need a loop where x takes values 0.1, 0.2, 0.3, …, 0.9, 1.0. We might be tempted to write something like this:

```python
x = 0.0
while not x == 1.0:
    x = x + 0.1
    print ( " x =%19.17f" % ( x ))
```

+++

However, this loop will never terminate. Here are the first 11 lines of output of the program:

    x=0.10000000000000001
    x=0.20000000000000001
    x=0.30000000000000004
    x=0.40000000000000002
    x=                0.5
    x=0.59999999999999998
    x=0.69999999999999996
    x=0.79999999999999993
    x=0.89999999999999991
    x=0.99999999999999989
    x=1.09999999999999987

+++

Because the variable `x` never takes exactly the value 1.0, the while loop will continue forever.

Thus: *Never compare two floating point numbers for equality.*

+++

### Using floating point numbers carefully 1

There are a number of alternative ways to solve this problem. For example, we can compare the distance between two floating point numbers:

```{code-cell} ipython3
x = 0.0
while abs(x - 1.0) > 1e-8:
    x = x + 0.1
    print ( " x =%19.17f" % ( x ))
```

### Using floating point numbers carefully 2

Alternatively, we can (for this example) iterate over a sequence of integers and compute the floating point number from the integer:

```{code-cell} ipython3
for i in range (1 , 11):
    x = i * 0.1
    print(" x =%19.17f" % ( x ))
```

```{code-cell} ipython3
x=0.10000000000000001
x=0.20000000000000001
x=0.30000000000000004
x=0.40000000000000002
x=                0.5
x=0.60000000000000009
x=0.70000000000000007
x=0.80000000000000004
x=0.90000000000000002
x=                  1
```

If we compare this with the output from the program in [Using floating point numbers (carelessly)](#Using-floating-point-numbers-&#40;carelessly&#41;), we can see that the floating point numbers differ. This means that – in a numerical calculation – it is not true that 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 = 1.0.

+++

### Symbolic calculation

Using the sympy package we have arbitrary precision. Using <span>`sympy.Rational`</span>, we can define the fraction 1/10 exactly symbolically. Adding this 10 times will lead exactly to the value 1, as demonstrated by this script

```{code-cell} ipython3
from sympy import Rational
dx = Rational (1 ,10)
x = 0
while x != 1.0:
    x = x + dx
    print("Current x=%4s = %3.1f " % (x , x . evalf ()))
    print(" Reached x=%s " % x)
```

However, this symbolic calculation is much slower as it is done through software rather than the CPU-based floating point operations. The next program approximates the relative performances:

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
from sympy import Rational
dx_symbolic = Rational (1 ,10)
dx = 0.1

def loop_sympy (n):
    x = 0
    for i in range(n):
        x = x + dx_symbolic
    return x

def loop_float(n):
    x =0
    for i in range(n):
        x = x + dx
    return x

def time_this (f, n):
    import time
    starttime = time.time()
    result = f(n)
    stoptime = time.time()
    print(" deviation is %16.15g" % ( n * dx_symbolic - result ))
    return stoptime - starttime

n = 100000
print("loop using float dx:")
time_float = time_this(loop_float, n)
print("float loop n=%d takes %6.5f seconds" % (n, time_float))
print("loop using sympy symbolic dx:")
time_sympy = time_this (loop_sympy, n)
print("sympy loop n =% d takes %6.5f seconds" % (n , time_sympy ))
print("Symbolic loop is a factor %.1f slower." % ( time_sympy / time_float ))
```

This is of course an artificial example: we have added the symbolic code to demonstrate that these round off errors originate from the approximative representation of floating point numbers in the hardware (and thus programming languages). We can, in principle, avoid these complications by computing using symbolic expressions, but this is in practice too slow.[4]

+++

### Summary

In summary, we have learned that

-   floating point numbers and integers used in numeric computation are generally quite different from “mathematical numbers” (symbolic calculations are exact and use the “mathematical numbers”):

    -   there is a maximum number and a minimum number that can be represented (for both integers and floating point numbers)

    -   within this range, not every floating point number can be represented in the computer.

-   We deal with this limitation by:

    -   never comparing two floating point numbers for equality (instead we compute the absolute value of the difference)

    -   use of algorithms that are *stable* (this means that small deviations from correct numbers can be corrected by the algorithm. We have not yet shown any such examples this document.)

-   Note that there is a lot more to be said about numerical and algorithmic tricks and methods to make numeric computation as accurate as possible but this is outside the scope of this section.

+++

### Exercise: infinite or finite loop

1.  What does the following piece of code compute? Will the loop ever finish? Why?

```python
eps = 1.0
while 1.0 + eps > 1.0:
    eps = eps / 2.0
print(eps)
```
