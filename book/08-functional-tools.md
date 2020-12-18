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

# Functional tools

Python provides a few in-built commands such as `map`, `filter`, `reduce` as
well `lambda` (to create anonymous functions) and list comprehension. These are
typical commands from functional languages of which LISP is probably best known.

Functional programming can be extremely powerful and one of the strengths of
Python is that it allows to program using (i) imperative/procedural programming
style, (ii) object oriented style and (iii) functional style. It is the
programmers choice which tools to select from which style and how to mix them to
best address a given problem.

In this chapter, we provide some examples for usage of the commands listed
above.

## Anonymous functions

All functions we have seen in Python so far have been defined through the `def`
keyword, for example:

```{code-cell} ipython3
def f(x):
     return x ** 2
```

This function has the name `f`. Once the function is defined (i.e. the Python
interpreter has come across the `def` line), we can call the function using its
name, for example

```{code-cell} ipython3
y = f(6)
```

Sometimes, we need to define a function that is only used once, or we want to
create a function but don’t need a name for it (as for creating closures). In
this case, this is called *anonymous* function as it does not have a name. In
Python, the `lambda` keyword can create an anonymous function.

We create a (named) function first, check it’s type and behaviour:

```{code-cell} ipython3
def f(x):
    return x ** 2

f
```

```{code-cell} ipython3
type(f)
```

```{code-cell} ipython3
f(10)
```

Now we do the same with an anonymous function:

```{code-cell} ipython3
lambda x: x ** 2
```

```{code-cell} ipython3
type(lambda x: x ** 2)
```

```{code-cell} ipython3
(lambda x: x ** 2)(10)
```

This works exactly in the same way but – as the anonymous function does not have
a name – we need to define the function (through the `lambda` expression) –
every time we need it.

Anonymous functions can take more than one argument:

```{code-cell} ipython3
(lambda x, y: x + y)(10, 20)
```

```{code-cell} ipython3
(lambda x, y, z: (x + y) * z )(10, 20, 2)
```

We will see some examples using `lambda` which will clarify typical use cases.

## Map

The map function `lst2 = map(f, s )` applies a function `f` to all elements in a
sequence `s`. The result of `map` can be turned into a list with the same length
as `s`:

```{code-cell} ipython3
def f(x):
    return x ** 2
lst2 = list(map(f, range(10)))
lst2
```

```{code-cell} ipython3
list(map(str.capitalize, ['banana', 'apple', 'orange']))
```

Often, this is combined with the anonymous function `lambda`:

```{code-cell} ipython3
list(map(lambda x: x ** 2, range(10) ))
```

```{code-cell} ipython3
list(map(lambda s: s.capitalize(), ['banana', 'apple', 'orange']))
```

## Filter

The filter function `lst2 = filter( f, lst)` applies the function `f` to all
elements in a sequence `s`. The function `f` should return `True` or `False`.
This makes a list which will contain only those elements $s_i$ of
the sequence `s` for which `f`($s_i$) has returned `True`.

```{code-cell} ipython3
def greater_than_5(x):
    if x > 5:
            return True
    else:
            return False

list(filter(greater_than_5, range(11)))
```

The usage of `lambda` can simplify this significantly:

```{code-cell} ipython3
list(filter(lambda x: x > 5, range(11)))
```

```{code-cell} ipython3
known_names = ['smith', 'miller', 'bob']
list(filter( lambda name : name in known_names, \
     ['ago', 'smith', 'bob', 'carl']))
```

## List comprehension

List comprehensions provide a concise way to create and modify lists without
resorting to use of map(), filter() and/or lambda. The resulting list definition
tends often to be clearer than lists built using those constructs. Each list
comprehension consists of an expression followed by a `for` clause, then zero or
more `for` or `if` clauses. The result will be a list resulting from evaluating
the expression in the context of the for and if clauses which follow it. If the
expression would evaluate to a tuple, it must be parenthesized.

Some examples will make this clearer:

```{code-cell} ipython3
freshfruit = ['  banana', '  loganberry ', 'passion fruit  ']
[weapon.strip() for weapon in freshfruit]
```

```{code-cell} ipython3
vec = [2, 4, 6]
[3 * x for x in vec]
```

```{code-cell} ipython3
[3 * x for x in vec if x > 3]
```

```{code-cell} ipython3
[3 * x for x in vec if x < 2]
```

```{code-cell} ipython3
[[x, x ** 2] for x in vec]
```

We can also use list comprehension to modify the list of integers returned by
the `range` command so that our subsequent elements in the list increase by
non-integer fractions:

```{code-cell} ipython3
[x*0.5 for x in range(10)]
```

Let’s now revisit the examples from the section on `filter`

```{code-cell} ipython3
[x for x in range(11) if x>5 ]
```

```{code-cell} ipython3
[name for name in ['ago','smith','bob','carl'] \
      if name in known_names]
```

and the examples from the `map` section

```{code-cell} ipython3
[x ** 2 for x in range(10) ]
```

```{code-cell} ipython3
[fruit.capitalize() for fruit in ['banana', 'apple', 'orange'] ]
```

all of which can be expressed through list comprehensions.

More details:
- Python Tutorial [5.1.4 List comprehensions](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)

## Reduce

The `reduce` function takes a binary function `f(x, y)`, a sequence `s`, and a
start value `a0`. It then applies the function `f` to the start value `a0` and
the first element in the sequence: `a1 = f(a0, s[0])`. The second element
(`s[1]`) of the sequence is then processed as follows: the function `f` is
called with arguments `a1` and `s[1]`, i.e. `a2 = f(a1, s[1])`. In this fashion,
the whole sequence is processed. Reduce returns a single element.

This can be used, for example, to compute a sum of numbers in a sequence if the
function `f(x, y)` returns `x+y`:

```{code-cell} ipython3
from functools import reduce
```

```{code-cell} ipython3
def add(x, y):
    return x + y

reduce(add, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 0)
```

```{code-cell} ipython3
reduce(add, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 100)
```

We can modify the function `add` to provide some more detail about the process:

```{code-cell} ipython3
def add_verbose(x, y):
    print("add(x=%s, y=%s) -> %s" % (x, y, x+y))
    return x+y

reduce(add_verbose, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 0)
```

It may be instructive to use an asymmetric function `f`, such as `add_len( n, s
)` where s is a sequence and the function returns `n+len(s)` (suggestion from
Thomas Fischbacher):

```{code-cell} ipython3
def add_len(n, s):
    return n + len(s)

reduce(add_len, ["This","is","a","test."],0)
```

As before, we’ll use a more verbose version of the binary function to see what
is happening:

```{code-cell} ipython3
def add_len_verbose(n, s):
    print("add_len(n=%d, s=%s) -> %d" % (n, s, n+len(s)))
    return n+len(s)

reduce(add_len_verbose, ["This", "is", "a", "test."], 0)
```

Another way to understand what the reduce function does is to look at the
following function (kindly provided by Thomas Fischbacher) which behaves like
`reduce` but explains what it does:

Here is an example using the `explain_reduce` function:

```{code-cell} ipython3
def explain_reduce(f, xs, start=None):
    """This function behaves like reduce, but explains what it does,
    step-by-step.
    (Author: Thomas Fischbacher, modifications Hans Fangohr)"""
    nr_xs = len(xs)
    if start == None:
        if nr_xs == 0:
            raise ValueError("No starting value given - cannot " + \
                              "process empty list!")
        if nr_xs == 1:
            print("reducing over 1-element list without starting " + \
                  "value: returning that element.")
            return xs[0]
        else:
            print("reducing over list with >= 2 elements without " +\
                  "starting value: using the first element as a " +\
                  "start value.")
            return explain_reduce(f, xs[1:], xs[0])
    else:
        s = start
        for n in range(len(xs)):
            x = xs[n]
            print("Step %d: value-so-far=%s next-list-element=%s"\
                  % (n, str(s), str(x)))
            s = f(s, x)
        print("Done. Final result=%s" % str(s))
        return s
```

```{code-cell} ipython3
def f(a, b):
    return a + b

reduce(f, [1, 2, 3, 4, 5], 0)
```

```{code-cell} ipython3
explain_reduce(f, [1, 2, 3, 4, 5], 0)
```

Reduce is often combined with `lambda`:

```{code-cell} ipython3
reduce(lambda x, y: x + y, [1, 2, 3, 4, 5], 0)
```

There is also the `operator` module which provides standard Python operators as
functions. For example, the function `operator.__add__(a,b)` is executed when
Python evaluates code such as `a+b`. These are generally faster than `lambda`
expressions. We could write the example above as

```{code-cell} ipython3
import operator
reduce(operator.__add__, [1, 2, 3, 4, 5], 0)
```

Use `help(’operator’)` to see the complete list of operator functions.

## Why not just use for-loops?

Let’s compare the example introduced at the beginning of the chapter written (i)
using a for-loop and (ii) list comprehension. Again, we want to compute the
numbers $0^2, 1^2, 2^2, 3^2, ...$ up to $(*n* − 1)^2$for a given $n$.

Implementation (i) using a for-loop with $n=10$:

```{code-cell} ipython3
y = []
for i in range(10):
    y.append(i**2)
```

Implementation (ii) using list comprehension:

```{code-cell} ipython3
y = [x**2 for x in range(10)]
```

or using `map`:

```{code-cell} ipython3
y = map(lambda x: x**2, range(10))
```

The versions using list comprehension and `map` fit into one line of code
whereas the for-loop needs 3. This example shows that functional code result in
very *concise* expressions. Typically, the number of mistakes a programmer makes
is per line of code written, so the fewer lines of code we have, the fewer bugs
we need to find.

Often programmers find that initially the list-processing tools introduced in
this chapter seem less intuitive than using for-loops to process every element
in a list individually, but that – over time – they come to value a more
functional programming style.

## Speed

The functional tools described in this chapter can also be faster than using
explicit (for or while) loops over list elements.

The program `list_comprehension_speed.py` below computes $\\sum\_{i=0}^{N-1}
i^2$ for a large value of *N* using 4 different methods and records execution
time:

- Method 1: for-loop (with pre-allocated list, storing of $i^2$ in list, then
  using in-built `sum` function)
- Method 2: for-loop without list (updating sum as the for-loop progresses)
- Method 3: using list comprehension
- Method 4: using numpy. (Numpy is covered in [chapter 14](14-numpy.ipynb))

Here is a possible program computing this:

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
"""Compare calculation of \sum_i x_i^2 with
i going from zero to N-1.

We use (i) for loops and list, (ii) for-loop, (iii) list comprehension
and (iv) numpy.

We use floating numbers to avoid using Python's long int (which would
be likely to make the timings less representative).
"""

import time
import numpy
N = 10000000


def timeit(f, args):
    """Given a function f and a tuple args containing
    the arguments for f, this function calls f(*args),
    and measures and returns the execution time in
    seconds.

    Return value is tuple: entry 0 is the time,
    entry 1 is the return value of f."""

    starttime = time.time()
    y = f(*args)    # use tuple args as input arguments
    endtime = time.time()
    return endtime - starttime, y


def forloop1(N):
    s = 0
    for i in range(N):
        s += float(i) * float(i)
    return s


def forloop2(N):
    y = [0] * N
    for i in range(N):
        y[i] = float(i) ** 2
    return sum(y)


def listcomp(N):
    return sum([float(x) * x for x in range(N)])


def numpy_(N):
    return numpy.sum(numpy.arange(0, N, dtype='d') ** 2)

# main program starts
timings = []
print("N =", N)
forloop1_time, f1_res = timeit(forloop1, (N,))
timings.append(forloop1_time)
print("for-loop1: {:5.3f}s".format(forloop1_time))
forloop2_time, f2_res = timeit(forloop2, (N,))
timings.append(forloop2_time)
print("for-loop2: {:5.3f}s".format(forloop2_time))
listcomp_time, lc_res = timeit(listcomp, (N,))
timings.append(listcomp_time)
print("listcomp : {:5.3f}s".format(listcomp_time))
numpy_time, n_res = timeit(numpy_, (N,))
timings.append(numpy_time)
print("numpy    : {:5.3f}s".format(numpy_time))

# ensure that different methods provide identical results
assert f1_res == f2_res
assert f1_res == lc_res

# Allow a bit of difference for the numpy calculation
numpy.testing.assert_approx_equal(f1_res, n_res)

print("Slowest method is {:.1f} times slower than the fastest method."
      .format(max(timings)/min(timings)))
```

The actual execution performance depends on the computer. The relative
performance may depend on versions of Python and its support libraries (such as
numpy) we use.

With the current version (python 3.6, numpy 1.11, on a x84 machine running OS
X), we see that methods 1 and 2 (for-loop without list and with pre-allocated
list) are slowest, somewhat closely followed by the slightly faster list
comprehension. The fastest method is number 4 (using numpy).

## The `%%timeit` magic

If we are using IPython as our shell (or a cell in a Jupyter notebook running a
python kernel), there is a much more sophisticated way to measure timings that
what is done above: if a cell starts with `%%timeit`, then IPython will run the
commands in that cell repeatedly and obtain (averaged) timings. This
particularly useful for timing of commands that execute relatively quickly.

Let's compare a list comprehension with an explicit loop using the `timeit`
magic:

```{code-cell} ipython3
%%timeit
y = [x**2 for x in range(100)]
```

```{code-cell} ipython3
%%timeit
y = []
for x in range(100):
    y.append(x**2)
```
