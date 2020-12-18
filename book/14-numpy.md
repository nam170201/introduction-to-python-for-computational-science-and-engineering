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

# Numerical Python (numpy): arrays

## Numpy introduction

The NumPy package (read as NUMerical PYthon) provides access to

- a new data structure called `array`s which allow
- efficient vector and matrix operations. It also provides
- a number of linear algebra operations (such as solving of systems of linear
  equations, computation of Eigenvectors and Eigenvalues).

### History

Some background information: There are two other implementations that provide
nearly the same functionality as NumPy. These are called “Numeric” and
“numarray”:

- Numeric was the first provision of a set of numerical methods (similar to
  Matlab) for Python. It evolved from a PhD project.
- Numarray is a re-implementation of Numeric with certain improvements (but for
  our purposes both Numeric and Numarray behave virtually identical).
- Early in 2006 it was decided to merge the best aspects of Numeric and Numarray
  into the Scientific Python (<span>`scipy`</span>) package and to provide (a
  hopefully “final”) `array` data type under the module name “NumPy”.

We will use in the following materials the “NumPy” package as provided by (new)
SciPy. If for some reason this doesn’t work for you, chances are that your SciPy
is too old. In that case, you will find that either “Numeric” or “numarray” is
installed and should provide nearly the same capabilities.[5]

### Arrays

We introduce a new data type (provided by NumPy) which is called “`array`”. An
array *appears* to be very similar to a list but an array can keep only elements
of the same type (whereas a list can mix different kinds of objects). This means
arrays are more efficient to store (because we don’t need to store the type for
every element). It also makes arrays the data structure of choice for numerical
calculations where we often deal with vectors and matrices.

Vectors and matrices (and matrices with more than two indices) are all called
“arrays” in NumPy.

#### Vectors (1d-arrays)

The data structure we will need most often is a vector. Here are a few examples
of how we can generate one:

- Conversion of a list (or tuple) into an array using `numpy.array`:

```{code-cell} ipython3
import numpy as N
x = N.array([0, 0.5, 1, 1.5])
print(x)
```

-   Creation of a vector using “ArrayRANGE”:

```{code-cell} ipython3
x = N.arange(0, 2, 0.5)
print(x)
```

-   Creation of vector with zeros

```{code-cell} ipython3
x = N.zeros(4)
print(x)
```

Once the array is established, we can set and retrieve individual values. For
example:

```{code-cell} ipython3
x = N.zeros(4)
x[0] = 3.4
x[2] = 4
print(x)
print(x[0])
print(x[0:-1])
```

Note that once we have a vector we can perform calculations on every element in
the vector with a single statement:

```{code-cell} ipython3
x = N.arange(0, 2, 0.5)
print(x)
print(x + 10)
print(x ** 2)
print(N.sin(x))
```

#### Matrices (2d-arrays)

Here are two ways to create a 2d-array:

- By converting a list of lists (or tuples) into an array:

```{code-cell} ipython3
x = N.array([[1, 2, 3], [4, 5, 6]])
x
```

- Using the `zeros` method to create a matrix with 5 rows and 4 columns

```{code-cell} ipython3
x = N.zeros((5, 4))
x
```

The “shape” of a matrix can be queried like this (here we have 2 rows and 3
columns):

```{code-cell} ipython3
x=N.array([[1, 2, 3], [4, 5, 6]])
print(x)
x.shape
```

Individual elements can be accessed and set using this syntax:

```{code-cell} ipython3
x=N.array([[1, 2, 3], [4, 5, 6]])
x[0, 0]
```

```{code-cell} ipython3
x[0, 1]
```

```{code-cell} ipython3
x[0, 2]
```

```{code-cell} ipython3
x[1, 0]
```

```{code-cell} ipython3
x[:, 0]
```

```{code-cell} ipython3
x[0,:]
```

### Convert from array to list or tuple

To create an array back to a list or tuple, we can use the standard python
functions `list(s)` and `tuple(s)` which take a sequence `s` as the input
argument and return a list and tuple, respectively:

```{code-cell} ipython3
a = N.array([1, 4, 10])
a
```

```{code-cell} ipython3
list(a)
```

```{code-cell} ipython3
tuple(a)
```

### Standard Linear Algebra operations

#### Maxtrix multiplication

Two arrays can be multiplied in the usual linear-algebra way using
`numpy.matrixmultiply`. Here is an example:

```{code-cell} ipython3
import numpy as N
import numpy.random
A = numpy.random.rand(5, 5)    # generates a random 5 by 5 matrix
x = numpy.random.rand(5)       # generates a 5-element vector
b=N.dot(A, x)                  # multiply matrix A with vector x
```

#### Solving systems of linear equations

To solve a system of equations $A \vec{x} = \vec{b}$ that is given in matrix
form (*i.e* $A$ is a matrix and $\vec{x}$ and $\vec{b}$ are vectors where $A$
and $\vec{b}$ are known and we want to find the unknown vector $\vec{x}$), we
can use the linear algebra package (`linalg`) of `numpy`:

```{code-cell} ipython3
import numpy.linalg as LA
x = LA.solve(A, b)
```

#### Computing Eigenvectors and Eigenvalues

Here is a small example that computes the \[trivial\] Eigenvectors and
Eigenvalues (`eig`) of the unity matrix (`eye`)):

```{code-cell} ipython3
import numpy
import numpy.linalg as LA
A = numpy.eye(3)     #'eye'->I->1 (ones on the diagonal)
print(A)
```

```{code-cell} ipython3
evalues, evectors = LA.eig(A)
print(evalues)
```

```{code-cell} ipython3
print(evectors)
```

Note that each of these commands provides its own documentation. For example,
`help(LA.eig)` will tell you all about the eigenvector and eigenvalue function
(once you have imported `numpy.linalg` as `LA`).

#### Curve fitting of polynomials

Let’s assume we have x-y data to which we like to fit a curve (to minimise the
least square deviation of the fit from the data).

Numpy provides the routine `polyfit(x,y,n)` (which is similar to Matlab’s
`polyfit` function which takes a list `x` of x-values for data points, a list
`y` of y-values of the same data points and a desired order of the polynomial
that will be determined to fit the data in the least-square sense as well as
possible.

```{code-cell} ipython3
%matplotlib inline
import numpy

# demo curve fitting : xdata and ydata are input data
xdata = numpy.array([0.0 , 1.0 , 2.0 , 3.0 , 4.0 , 5.0])
ydata = numpy.array([0.0 , 0.8 , 0.9 , 0.1 , -0.8 , -1.0])
# now do fit for cubic (order = 3) polynomial
z = numpy.polyfit(xdata, ydata, 3)
# z is an array of coefficients , highest first , i . e .
#                 X^3            X^2          X             0
# z = array ([ 0.08703704 , -0.81349206 , 1.69312169 , -0.03968254])
# It is convenient to use ‘poly1d‘ objects for dealing with polynomials:
p = numpy.poly1d(z) # creates a polynomial function p from coefficients
                    # and p can be evaluated for all x then .

# create plot
xs = [0.1 * i for i in range (50)]
ys = [p ( x ) for x in xs]   # evaluate p(x) for all x in list xs

import pylab
pylab.plot(xdata, ydata, 'o', label='data')
pylab.plot(xs, ys, label='fitted curve')
pylab.ylabel('y')
pylab.xlabel('x')
```

This shows the fitted curve (solid line) together with the precise computed data
points.

### More numpy examples…

…can be found here: <https://numpy.org/doc/stable/reference/routines.html>

### Numpy for Matlab users

There is a dedicated webpage that explains Numpy from the perspective of a
(experienced) Matlab user at
https://numpy.org/doc/stable/user/numpy-for-matlab-users.html.
