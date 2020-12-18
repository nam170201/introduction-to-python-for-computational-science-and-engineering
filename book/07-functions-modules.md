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

# Functions and modules

## Introduction

Functions allow us to group a number of statements into a logical block. We
communicate with a function through a clearly defined interface, providing
certain parameters to the function, and receiving some information back. Apart
from this interface, we generally do not how exactly a function does the work to
obtain the value it returns

For example the function `math.sqrt`: we do not know how exactly it computes the
square root, but we know about the interface: if we pass *x* into the function,
it will return (an approximation of) $\sqrt{x}$.

This abstraction is a useful thing: it is a common technique in engineering to
break down a system into smaller (black-box) components that all work together
through well defined interfaces, but which do not need to know about the
internal realisations of each other’s functionality. In fact, not having to care
about these implementation details can help to have a clearer view of the system
composed of many of these components.

Functions provide the basic building blocks of functionality in larger programs
(and computer simulations), and help to control the inherent complexity of the
process.

We can group functions together into a Python module (see [modules](#Modules)),
and in this way create our own libraries of functionality.

## Using functions

The word “function” has different meanings in mathematics and programming. In
programming it refers to a named sequence of operations that perform a
computation. For example, the function `sqrt()` which is defined in the `math`
module computes the square root of a given value:

```{code-cell}
from math import sqrt

sqrt(4)
```

The value we pass to the function `sqrt` is 4 in this example. This value is
called the *argument* of the function. A function may have more than one
argument.

The function returns the value 2.0 (the result of its computation) to the
“calling context”. This value is called the *return value* of the function.

It is common to say that a function *takes* an argument and *returns* a result
or return value.

**Common confusion about printing and returning values**

It is a common beginner’s mistake to confuse the *printing* of values with
*returning* values. In the following example it is hard to see whether the
function `math.sin` returns a value or whether it prints the value:

```{code-cell}
import math

math.sin(2)
```

We import the `math` module, and call the `math.sin` function with an argument
of `2`. The `math.sin(2)` call will actually *return* the value `0.909...` not
print it. However, because we have not assigned the return value to a variable,
the Python prompt will print the returned object.

The following alternative sequence works only if the value is returned:

```{code-cell}
x = math.sin(2)
print(x)
```

The return value of the function call `math.sin(2)` is assigned to the variable
`x`, and `x` is printed in the next line.

Generally, functions should execute “silently” (i.e. not print anything) and
report the result of their computation through the return value.

Part of the confusion about printed versus return values at the Python prompt
comes from the Python prompt printing (a representation) of returned objects
*if* the returned objects are not assigned. Generally, seeing the returned
objects is exactly what we want (as we normally care about the returned object),
just when learning Python this may cause mild confusion about functions
returning values or printing values.

Further information:
- Think Python has a gentle introduction to functions (on which the previous
  paragraph is based) in [chapter 3
  (Functions)](https://www.greenteapress.com/thinkpython/html/book004.html) and
  [chapter 6 (Fruitful
  functions)](https://www.greenteapress.com/thinkpython/html/book007.html).

## Defining functions

The generic format of a function definitions:

```python
def my_function(arg1, arg2, ..., argn):
    """Optional docstring."""

    # Implementation of the function

    return result  # optional

# this is not part of the function
some_command
```

Allen Downey’s terminology (in his book [Think
Python](https://www.greenteapress.com/thinkpython/html/index.html)) of fruitful
and fruitless functions distinguishes between functions that return a value, and
those that do not return a value. The distinction refers to whether a function
provides a return value (=fruitful) or whether the function does not explicitly
return a value (=fruitless). If a function does not make use of the `return`
statement, we tend to say that the function returns nothing (whereas in reality
in will always return the `None` object when it terminates – even if the
`return` statement is missing).

For example, the function `greeting` will print “Hello World” when called (and
is fruitless as it does not return a value).

```{code-cell}
def greeting():
    print("Hello World!")
```

If we call that function:

```{code-cell}
greeting()
```

it prints “Hello World” to stdout, as we would expect. If we assign the return
value of the function to a variable `x`, we can inspect it subsequently:

```{code-cell}
x = greeting()
```

```{code-cell}
print(x)
```

and find that the `greeting` function has indeed returned the `None` object.

Another example for a function that does not return any value (that means there
is no `return` keyword in the function) would be:

```{code-cell}
def printpluses(n):
    print(n * "+")
```

Generally, functions that return values are more useful as these can be used to
assemble code (maybe as another function) by combining them cleverly. Let’s look
at some examples of functions that do return a value.

Suppose we need to define a function that computes the square of a given
variable. The function source could be:

```{code-cell}
def square(x):
    return x * x
```

The keyword `def` tells Python that we are *defining* a function at that point.
The function takes one argument (`x`). The function returns `x*x` which is of
course $x^2$. Here is the listing of a file that shows how the function can be
defined and used: (note that the numbers on the left are line numbers and are
not part of the program)

```{code-cell}
---
attributes:
  classes: [numberLines]
  id: ''
---
def square(x):
    return x * x


for i in range(5):
    i_squared = square(i)
    print(i, "*", i, "=", i_squared)
```

It is worth mentioning that lines 1 and 2 define the square function whereas
lines 4 to 6 are the main program.

We can define functions that take more than one argument:

```{code-cell}
import math


def hypot(x, y):
    return math.sqrt(x * x + y * y)
```

It is also possible to return more than one argument. Here is an example of a
function that converts a given string into all characters uppercase and all
characters lowercase and returns the two versions. We have included the main
program to show how this function can be called:

```{code-cell}
def upperAndLower(string):
    return string.upper(), string.lower()


testword = "Banana"

uppercase, lowercase = upperAndLower(testword)

print(testword, "in lowercase:", lowercase, "and in uppercase", uppercase)
```

We can define multiple Python functions in one file. Here is an example with two
functions:

```{code-cell}
def returnstars(n):
    return n * '*'


def print_centred_in_stars(string):
    linelength = 46
    starstring = returnstars((linelength - len(string)) // 2)

    print(starstring + string + starstring)


print_centred_in_stars("Hello world!")
```

Further reading:
- [Python Tutorial: Section 4.6 Defining Functions](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)

## Default values and optional parameters

Python allows to define *default* values for function parameters. Here is an
example: This program will print the following output when executed: So how does
it work? The function `print_mult_table` takes two arguments: `n` and `upto`.
The first argument `n` is a “normal” variable. The second argument `upto` has a
default value of 10. In other words: should the user of this function only
provide one argument, then this provides the value for `n` and `upto` will
default to 10. If two arguments are provided, the first one will be for `n` and
the second for `upto` (as shown in the code example above).


## Modules

Modules

- Group together functionality
- Provide namespaces
- Python’s standard library contains a vast collection of modules - “Batteries
  Included”
  - Try `help(’modules’)`
- Means of extending Python

### Importing modules

```{code-cell}
import math
```

This will introduce the name `math` into the namespace in which the import
command was issued. The names within the `math` module will not appear in the
enclosing namespace: they must be accessed through the name `math`. For example:
`math.sin`.

```{code-cell}
import math, cmath
```

More than one module can be imported in the same statement, although the [Python
Style Guide](https://www.python.org/dev/peps/pep-0008/) recommends not to do
this. Instead, we should write

```{code-cell}
import math
import cmath

import math as mathematics
```

The name by which the module is known locally can be different from its
“official” name. Typical uses of this are

- To avoid name clashes with existing names
- To change the name to something more manageable. For example `import
  SimpleHTTPServer as shs`. This is discouraged for production code (as longer
  meaningful names make programs far more understandable than short cryptic
  ones), but for interactively testing out ideas, being able to use a short
  synonym can make your life much easier. Given that (imported) modules are
  first class objects, you can, of course, simply do `shs = SimpleHTTPServer` in
  order to obtain the more easily typable handle on the module.

```{code-cell}
from math import sin
```

This will import the `sin` function from the `math` module, but it will not
introduce the name math into the current namespace. It will only introduce the
name `sin` into the current namespace. It is possible to pull in more than one
name from the module in one go:

```{code-cell}
from math import sin, cos
```

Finally, let’s look at this notation:

```{code-cell}
from math import *
```

Once again, this does not introduce the name math into the current namespace. It
does however introduce *all public names* of the math module into the current
namespace. Broadly speaking, it is a bad idea to do this:

- Lots of new names will be dumped into the current namespace.
- Are you sure they will not clobber any names already present?
- It will be very difficult to trace where these names came from
- Having said that, some modules (including ones in the standard library,
  recommend that they be imported in this way). Use with caution!
- This is fine for interactive quick and dirty testing or small calculations.

### Creating modules

A module is in principle nothing else than a python file. We create an example
of a module file which is saved in `module1.py`:

```{code-cell}
%%file module1.py
def someusefulfunction():
    pass


print("My name is", __name__)
```

We can execute this (module) file as a normal python program (for example
`python module1.py`):

```{code-cell}
!python3 module1.py
```

We note that the Python magic variable `__name__` takes the value `__main__` if
the program file `module1.py` is executed.

On the other hand, we can *import* `module1.py` in another file (which could
have the name `prog.py`), for example like this:

```{code-cell}
import module1  # in file prog.py
```

When Python comes across the `import module1` statement in `prog.py`, it looks
for the file `module1.py` in the current working directory (and if it can’t find
it there in all the directories in `sys.path`) and opens the file `module1.py`.
While parsing the file `module1.py` from top to bottom, it will add any function
definitions in this file into the `module1` name space in the calling context
(that is the main program in `prog.py`). It this example, there is only the
function `someusefulfunction`. Once the import process is completed, we can make
use of `module1.someusefulfunction` in `prog.py`. If Python comes across
statements other than function (and class) definitions while importing
`module1.py`, it carries those out immediately. In this case, it will thus come
across the statement `print(My name is, __name__)`.

Note the difference to the output if we *import* `module1.py` rather than
executing it on its own: `__name__` inside a module takes the value of the
module name if the file is imported.

### Use of `__name__`

In summary,

- `__name__` is `__main__` if the module file is run on its own
- `__name__` is the name of the module (i.e. the module filename without the
  `.py` suffix) if the module file is imported.

We can therefore use the following `if` statement in `module1.py` to write code
that is *only run* when the module is executed on its own: This is useful to
keep test programs or demonstrations of the abilities of a module in this
“conditional” main program. It is common practice for any module files to have
such a conditional main program which demonstrates its capabilities.

### Example 1

The next example shows a main program for the another file `vectools.py` that is
used to demonstrate the capabilities of the functions defined in that file:

```{code-cell}
%%file vectools.py
from __future__ import division
import math

import numpy as N


def norm(x):
    """returns the magnitude of a vector x"""
    return math.sqrt(sum(x ** 2))


def unitvector(x):
    """returns a unit vector x/|x|. x needs to be a numpy array."""
    xnorm = norm(x)
    if xnorm == 0:
        raise ValueError("Can't normalise vector with length 0")
    return x / norm(x)


if __name__ == "__main__":
    # a little demo of how the functions in this module can be used:
    x1 = N.array([0, 1, 2])
    print("The norm of " + str(x1) + " is " + str(norm(x1)) + ".")
    print(
        "The unitvector in direction of " + str(x1) + " is " + str(unitvector(x1)) + "."
    )
```

If this file is executed using `python vectools.py`, then `__name__==__main__`
is true, and the output reads

```{code-cell}
!python3 vectools.py
```

If this file is imported (i.e. used as a module) into another python file or the
python prompt or in the Jupyter Notebook, then `__name__==__main__` is false,
and that statement block will not be executed.

This is quite a common way to conditionally execute code in files providing
library-like functions. The code that is executed if the file is run on its own,
often consists of a series of tests (to check that the file’s functions carry
out the right operations – *regression tests* or *unit tests* ), or some
examples of how the library functions in the file can be used.

### Example 2

Even if a Python program is not intended to be used as a module file, it is good
practice to always use a conditional main program:

- often, it turns out later that functions in the file can be reused (and saves
  work then)
- this is convenient for regression testing.

Suppose an exercise is given to write a function that returns the first 5 prime
numbers, and in addition to print them. (There is of course a trivial solution
to this as we know the prime numbers, and we should imagine that the required
calculation is more complex). One might be tempted to write

```{code-cell}
def primes5():
    return (2, 3, 5, 7, 11)


for p in primes5():
    print("%d" % p, end=" ")
```

It is better style to use a conditional main function, i.e.:

```{code-cell}
def primes5():
    return (2, 3, 5, 7, 11)


if __name__ == "__main__":
    for p in primes5():
        print("%d" % p, end=" ")
```

A purist might argue that the following is even cleaner:

```{code-cell}
def primes5():
    return (2, 3, 5, 7, 11)


def main():
    for p in primes5():
        print('%d' % p, end=' ')


if __name__ == '__main__':
    main()
```

but either of the last two options is good.

The example in [Many ways to compute a series](#Many-ways-to-compute-a-series)
demonstrates this technique. Including functions with names starting with
`test_` is compatible with the very useful py.test regression testing framework
(see <https://docs.pytest.org/en/stable//>).

## Further Reading

- [Python Tutorial Section 6](https://docs.python.org/3/tutorial/modules.html#modules)
