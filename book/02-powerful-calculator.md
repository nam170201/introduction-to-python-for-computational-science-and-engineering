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

# A powerful calculator

## Python prompt and Read-Eval-Print Loop (REPL)

Python is an *interpreted* language. We can collect sequences of commands into text files and save this to file as a *Python program*. It is convention that these files have the file extension “`.py`”, for example `hello.py`.

We can also enter individual commands at the Python prompt which are immediately evaluated and carried out by the Python interpreter. This is very useful for the programmer/learner to understand how to use certain commands (often before one puts these commands together in a longer Python program). Python’s role can be described as Reading the command, Evaluating it, Printing the evaluated value and repeating (Loop) the cycle – this is the origin of the REPL abbreviation.

Python comes with a basic terminal prompt; you may see examples from this with `>>>` marking the input:


    >>> 2 + 2
    4

We are using a more powerful REPL interface, the Jupyter Notebook. Blocks of code appear with an `In` prompt next to them:

```{code-cell} ipython3
4 + 5
```

To edit the code, click inside the code area. You should get a green border around it. To run it, press Shift-Enter.

+++

## Calculator

Basic operations such as addition (`+`), subtraction (`-`), multiplication (`*`), division (`/`) and exponentiation (`**`) work (mostly) as expected:

```{code-cell} ipython3
10 + 10000
```

```{code-cell} ipython3
42 - 1.5
```

```{code-cell} ipython3
47 * 11
```

```{code-cell} ipython3
10 / 0.5
```

```{code-cell} ipython3
2**2   # Exponentiation ('to the power of') is **, NOT ^
```

```{code-cell} ipython3
2**3
```

```{code-cell} ipython3
2**4
```

```{code-cell} ipython3
2 + 2
```

```{code-cell} ipython3
# This is a comment
2 + 2
```

```{code-cell} ipython3
2 + 2  # and a comment on the same line as code
```

and, using the fact that $\sqrt[n]{x} = x^{1/n}$, we can compute the $\sqrt{3} = 1.732050\dots$ using `**`:

```{code-cell} ipython3
3**0.5
```

Parenthesis can be used for grouping:

```{code-cell} ipython3
2 * 10 + 5
```

```{code-cell} ipython3
2 * (10 + 5)
```

## Integer division

In Python 3, division works as you'd expect:

```{code-cell} ipython3
15/6
```

In Python 2, however, `15/6` will give you `2`.


This phenomenon is known (in many programming languages, including C) as *integer division*: because we provide two integer numbers (`15` and `6`) to the division operator (`/`), the assumption is that we seek a return value of type integer. The mathematically correct answer is (the floating point number) 2.5. (→ numerical data types in [Chapter 13](13-numeric-computation.ipynb).)

The convention for integer division is to truncate the fractional digits and to return the integer part only (i.e. `2` in this example). It is also called “floor division”.

### How to avoid integer division

There are two ways to avoid the problem of integer division:

1. Use Python 3 style division: this is available even in Python 2 with a special import statement:

   ```python
   >>> from __future__ import division
   >>> 15/6
   2.5
   ```

   If you want to use the `from __future__ import division` feature in a python program, it would normally be included at the beginning of the file.

2.  Alternatively, if we ensure that at least one number (numerator or denominator) is of type float (or complex), the division operator will return a floating point number. This can be done by writing `15.` instead of `15`, of by forcing conversion of the number to a float, i.e. use `float(15)` instead of `15`:

   ```python
   >>> 15./6
   2.5
   >>> float(15)/6
   2.5
   >>> 15/6.
   2.5
   >>> 15/float(6)
   2.5
   >>> 15./6.
   2.5
   ```

If we really want integer division, we can use `//`: `1//2` returns 0, in both Python 2 and 3.

+++

### Why should I care about this division problem?

Integer division can result in surprising bugs: suppose you are writing code to compute the mean value *m* = (*x* + *y*)/2 of two numbers *x* and *y*. The first attempt of writing this may read:

```python
m = (x + y) / 2
```

Suppose this is tested with *x* = 0.5, *y* = 0.5, then the line above computes the correct answers *m* = 0.5 (because`0.5 + 0.5 = 1.0`, i.e. a 1.0 is a floating point number, and thus `1.0/2` evaluates to `0.5`). Or we could use *x* = 10, *y* = 30, and because `10 + 30 = 40` and `40/2` evaluates to `20`, we get the correct answer *m* = 20. However, if the integers *x* = 0 and *y* = 1 would come up, then the code returns *m* = 0 (because `0 + 1 = 1` and `1/2` evaluates to `0`) whereas *m* = 0.5 would have been the right answer.

We have many possibilities to change the line of code above to work safely, including these three versions:

```python
m = (x + y) / 2.0

m = float(x + y) / 2

m = (x + y) * 0.5
```

This integer division behaviour is common amongst most programming languages (including the important ones C, C++ and Fortran), and it is important to be aware of the issue.

## Mathematical functions

Because Python is a general purpose programming language, commonly used mathematical functions such as sin, cos, exp, log and many others are located in the mathematics module with name `math`. We can make use of this as soon as we *import* the math module:

```{code-cell} ipython3
import math
math.exp(1.0)
```

Using the `dir` function, we can see the directory of objects available in the math module:

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
dir(math)
```

As usual, the `help` function can provide more information about the module (`help(math)`) on individual objects:

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
help(math.exp)
```

The mathematics module defines to constants *π* and *e*:

```{code-cell} ipython3
math.pi
```

```{code-cell} ipython3
math.e
```

```{code-cell} ipython3
math.cos(math.pi)
```

```{code-cell} ipython3
math.log(math.e)
```

## Variables

A *variable* can be used to store a certain value or object. In Python, all numbers (and everything else, including functions, modules and files) are objects. A variable is created through assignement:

```{code-cell} ipython3
---
jupyter:
  outputs_hidden: true
---
x = 0.5
```

Once the variable `x` has been created through assignment of 0.5 in this example, we can make use of it:

```{code-cell} ipython3
x*3
```

```{code-cell} ipython3
x**2
```

```{code-cell} ipython3
y = 111
y + 222
```

A variable is overriden if a new value is assigned:

```{code-cell} ipython3
y = 0.7
math.sin(y) ** 2 + math.cos(y) ** 2
```

The equal sign (’=’) is used to assign a value to a variable.

```{code-cell} ipython3
width = 20
height = 5 * 9
width * height
```

A value can be assigned to several variables simultaneously:

```{code-cell} ipython3
x = y = z = 0  # initialise x, y and z with 0
x
```

```{code-cell} ipython3
y
```

```{code-cell} ipython3
z
```

Variables must be created (assigned a value) before they can be used, or an error will occur:

```{code-cell} ipython3
:tags: [raises-exception]

# NBVAL_RAISES_EXCEPTION
# try to access an undefined variable:
n
```

In interactive mode, the last printed expression is assigned to the variable `_`. This means that when you are using Python as a desk calculator, it is somewhat easier to continue calculations, for example:

```{code-cell} ipython3
tax = 12.5 / 100
price = 100.50
price * tax
```

```{code-cell} ipython3
price + _
```

This variable should be treated as read-only by the user. Don’t explicitly assign a value to it — you would create an independent local variable with the same name masking the built-in variable with its magic behavior.

### Terminology

Strictly speaking, the following happens when we write

```{code-cell} ipython3
---
jupyter:
  outputs_hidden: true
---
x = 0.5
```

First, Python creates the object `0.5`. Everything in Python is an object, and so is the floating point number 0.5. This object is stored somewhere in memory. Next, Python *binds a name to the object*. The name is `x`, and we often refer casually to `x` as a variable, an object, or even the value 0.5. However, technically, `x` is a name that is bound to the object `0.5`. Another way to say this is that `x` is a reference to the object.

While it is often sufficient to think about assigning 0.5 to a variable x, there are situations where we need to remember what actually happens. In particular, when we pass references to objects to functions, we need to realise that the function may operate on the object (rather than a copy of the object). This is discussed in more detail in [the next chapter](03-data-types-structures.ipynb).

## Impossible equations

In computer programs we often find statements like

```{code-cell} ipython3
---
jupyter:
  outputs_hidden: true
---
x = x + 1
```

If we read this as an equation as we are use to from mathematics,
*x* = *x* + 1
 we could subtract *x* on both sides, to find that
0 = 1.
 We know this is not true, so something is wrong here.

The answer is that “equations“ in computer codes are not equations but *assignments*. They always have to be read in the following way two-step way:

1.  Evaluate the value on the right hand side of the equal sign

2.  Assign this value to the variable name shown on the left hand side. (In Python: bind the name on the left hand side to the object shown on the right hand side.)

Some computer science literature uses the following notation to express assignments and to avoid the confusion with mathematical equations:

$$x \leftarrow x + 1$$

Let’s apply our two-step rule to the assignment `x = x + 1` given above:

1.  Evaluate the value on the right hand side of the equal sign: for this we need to know what the current value of `x` is. Let’s assume `x` is currently `4`. In that case, the right hand side `x+1` evaluates to `5`.

2.  Assign this value (i.e. `5`) to the variable name shown on the left hand side `x`.

Let’s confirm with the Python prompt that this is the correct interpretation:

```{code-cell} ipython3
x = 4     
x = x + 1
x
```

### The `+=` notation

Because it is a quite a common operation to increase a variable `x` by some fixed amount `c`, we can write

```python
x += c
```

instead of

```python
x = x + c
```

+++

Our initial example above could thus have been written

```{code-cell} ipython3
x = 4
x += 1
x
```

The same operators are defined for multiplication with a constant (`*=`), subtraction of a constant (`-=`) and division by a constant (`/=`).

Note that the order of `+` and `=` matters:

```{code-cell} ipython3
---
jupyter:
  outputs_hidden: true
---
x += 1
```

will increase the variable `x` by one where as

```{code-cell} ipython3
---
jupyter:
  outputs_hidden: true
---
x =+ 1
```

will assign the value `+1` to the variable `x`.
