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

# Data Types and Data Structures

## What type is it?

Python knows different data types. To find the type of a variable, use the
`type()` function:

```{code-cell}
a = 45
type(a)
```

```{code-cell}
b = "This is a string"
type(b)
```

```{code-cell}
c = 2 + 1j
type(c)
```

```{code-cell}
d = [1, 3, 56]
type(d)
```

## Numbers

**Further information**

- Informal introduction to numbers. [Python tutorial, section
  3.1.1](https://docs.python.org/3/tutorial/introduction.html#using-python-as-a-calculator)
- Python Library Reference: formal overview of numeric types,
  <https://docs.python.org/3.8/library/stdtypes.html#numeric-types-int-float-complex>
- Think Python, [Sec
  2.1](https://www.greenteapress.com/thinkpython/html/book003.html)

The in-built numerical types are integers and floating point numbers (see
[floating point numbers](#Floating-Point-numbers)) and complex floating point
numbers ([complex numbers](#Complex-numbers)).

### Integers

We have seen the use of integer numbers already in [Chapter
2](02-powerful-calculator.ipynb). Be aware of integer division problems (02 A
Powerful Calculator, Integer Division).

If we need to convert string containing an integer number to an integer we can
use `int()` function:

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
a = '34'  # a is a string containing the characters 3 and 4
x = int(a)  # x is in integer number
```

The function `int()` will also convert floating point numbers to integers:

```{code-cell}
int(7.0)
```

```{code-cell}
int(7.9)
```

Note than `int` will truncate any non-integer part of a floating point number.
To `round` an floating point number to an integer, use the `round()` command:

```{code-cell}
round(7.9)
```

### Integer limits

Integers in Python 3 are unlimited; Python will automatically assign more memory
as needed as the numbers get bigger. This means we can calculate very large
numbers with no special steps.

```{code-cell}
35 ** 42
```

In many other programming languages, such as C and FORTRAN, integers are a fixed
size—most frequently 4 bytes, which allows $2^{32}$ different values—but
different types are available with different sizes. For numbers that fit into
these limits, calculations can be faster, but you may need to check that the
numbers don't go beyond the limits. Calculating a number beyond the limits is
called *integer overflow*, and may produce bizarre results.

Even in Python, we need to be aware of this when we use numpy (see [Chapter
14](14-numpy.ipynb)). Numpy uses integers with a fixed size, because it stores
many of them together and needs to calculate with them efficiently. [Numpy data
types](https://numpy.org/doc/stable/user/basics.types.html) include a range of
integer types named for their size, so e.g. `int16` is a 16-bit integer, with
$2^{16}$ possible values.

Integer types can also be *signed* or *unsigned*. Signed integers allow positive
or negative values, unsigned integers only allow positive ones. For instance:

- uint16 (unsigned) ranges from 0 to $2^{16}-1$
- int16 (signed) ranges from $-2^{15}$ to $2^{15}-1$

+++

### Floating Point numbers

A string containing a floating point number can be converted into a floating
point number using the `float()` command:

```{code-cell}
a = '35.342'
b = float(a)
b
```

```{code-cell}
type(b)
```

### Complex numbers

Python (as Fortran and Matlab) has built-in complex numbers. Here are some
examples how to use these:

```{code-cell}
x = 1 + 3j
x
```

```{code-cell}
abs(x)  # computes the absolute value
```

```{code-cell}
x.imag
```

```{code-cell}
x.real
```

```{code-cell}
x * x
```

```{code-cell}
x * x.conjugate()
```

```{code-cell}
3 * x
```

Note that if you want to perform more complicated operations (such as taking the
square root, etc) you have to use the `cmath` module (Complex MATHematics):

```{code-cell}
import cmath

cmath.sqrt(x)
```

### Functions applicable to all types of numbers

The `abs()` function returns the absolute value of a number (also called
modulus):

```{code-cell}
a = -45.463
abs(a)
```

Note that `abs()` also works for complex numbers (see above).

## Sequences

Strings, lists and tuples are *sequences*. They can be *indexed* and *sliced* in
the same way.

Tuples and strings are “immutable” (which basically means we can’t change
individual elements within the tuple, and we cannot change individual characters
within a string) whereas lists are “mutable” (*.i.e* we can change elements in a
list.)

Sequences share the following operations

- `a[i]` returns i-th element of `a`
- `a[i:j]` returns elements i up to j-1
- `len(a)` returns number of elements in sequence
- `min(a)` returns smallest value in sequence
- `max(a)` returns largest value in sequence
- `x in a` returns `True` if `x` is element in `a`
- `a + b` concatenates `a` and `b`
- `n * a` creates `n` copies of sequence `a`

+++

### Sequence type 1: String

- Introduction to strings, [Python tutorial
  3.1.2](https://docs.python.org/3/tutorial/introduction.html#strings)

A string is a (immutable) sequence of characters. A string can be defined using
single quotes:

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
a = "Hello World"
```

double quotes:

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
a = "Hello World"
```

or triple quotes of either kind

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
a = """Hello World"""
a = """Hello World"""
```

The type of a string is `str` and the empty string is given by `""`:

```{code-cell}
a = "Hello World"
type(a)
```

```{code-cell}
b = ""
type(b)
```

```{code-cell}
type("Hello World")
```

```{code-cell}
type("")
```

The number of characters in a string (that is its *length*) can be obtained
using the `len()`-function:

```{code-cell}
a = "Hello Moon"
len(a)
```

```{code-cell}
a = "test"
len(a)
```

```{code-cell}
len("another test")
```

You can combine (“concatenate”) two strings using the `+` operator:

```{code-cell}
"Hello " + "World"
```

Strings have a number of useful methods, including for example `upper()` which
returns the string in upper case:

```{code-cell}
a = "This is a test sentence."
a.upper()
```

A list of available string methods can be found in the Python reference
documentation. If a Python prompt is available, one should use the `dir` and
`help` function to retrieve this information, *i.e.* `dir()` provides the list
of methods, `help` can be used to learn about each method.

A particularly useful method is `split()` which converts a string into a list of
strings:

```{code-cell}
a = "This is a test sentence."
a.split()
```

The `split()` method will separate the string where it finds *white space*.
White space means any character that is printed as white space, such as one
space or several spaces or a tab.

By passing a separator character to the `split()` method, a string can split
into different parts. Suppose, for example, we would like to obtain a list of
complete sentences:

```{code-cell}
a = "The dog is hungry. The cat is bored. The snake is awake."
a.split(".")
```

The opposite string method to `split` is `join` which can be used as follows:

```{code-cell}
a = "The dog is hungry. The cat is bored. The snake is awake."
s = a.split('.')
s
```

```{code-cell}
".".join(s)
```

```{code-cell}
" STOP".join(s)
```

### Sequence type 2: List

**Further information**

- Introduction to Lists, [Python tutorial, section
  3.1.4](https://docs.python.org/3/tutorial/introduction.html#lists)

A list is a sequence of objects. The objects can be of any type, for example
integers:

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
a = [34, 12, 54]
```

or strings:

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
a = ['dog', 'cat', 'mouse']
```

An empty list is presented by `[]`:

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
a = []
```

The type is `list`:

```{code-cell}
type(a)
```

```{code-cell}
type([])
```

As with strings, the number of elements in a list can be obtained using the
`len()` function:

```{code-cell}
a = ['dog', 'cat', 'mouse']
len(a)
```

It is also possible to *mix* different types in the same list:

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
a = [123, 'duck', -42, 17, 0, 'elephant']
```

In Python a list is an object. It is therefore possible for a list to contain
other lists (because a list keeps a sequence of objects):

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
a = [1, 4, 56, [5, 3, 1], 300, 400]
```

You can combine (“concatenate”) two lists using the `+` operator:

```{code-cell}
[3, 4, 5] + [34, 35, 100]
```

Or you can add one object to the end of a list using the `append()` method:

```{code-cell}
a = [34, 56, 23]
a.append(42)
a
```

You can delete an object from a list by calling the `remove()` method and
passing the object to delete. For example:

```{code-cell}
a = [34, 56, 23, 42]
a.remove(56)
a
```

#### The range() command

A special type of list is frequently required (often together with `for-loops`)
and therefore a command exists to generate that list: the `range(n)` command
generates integers starting from 0 and going up to *but not including* n. Here
are a few examples:

```{code-cell}
list(range(3))
```

```{code-cell}
list(range(10))
```

This command is often used with for loops. For example, to print the numbers
$0^2,1^2,2^2,3^2,…,10^2$, the following program can be used:

```{code-cell}
for i in range(11):
    print(i ** 2)
```

The range command takes an optional parameter for the beginning of the integer
sequence (start) and another optional parameter for the step size. This is often
written as `range([start],stop,[step])` where the arguments in square brackets
(*i.e.* start and step) are optional. Here are some examples:

```{code-cell}
list(range(3, 10))  # start=3
```

```{code-cell}
list(range(3, 10, 2))  # start=3, step=2
```

```{code-cell}
list(range(10, 0, -1))  # start=10,step=-1
```

Why are we calling `list(range())`?

In Python 3, `range()` generates the numbers on demand. When you use `range()`
in a for loop, this is more efficient, because it doesn't take up memory with a
list of numbers. Passing it to `list()` forces it to generate all of its
numbers, so we can see what it does.

To get the same efficient behaviour in Python 2, use `xrange()` instead of
`range()`.

### Sequence type 3: Tuples

A *tuple* is a (immutable) sequence of objects. Tuples are very similar in
behaviour to lists with the exception that they cannot be modified (i.e. are
immutable).

For example, the objects in a sequence can be of any type:

```{code-cell}
a = (12, 13, 'dog')
a
```

```{code-cell}
a[0]
```

The parentheses are not necessary to define a tuple: just a sequence of objects
separated by commas is sufficient to define a tuple:

```{code-cell}
a = 100, 200, 'duck'
a
```

although it is good practice to include the parenthesis where it helps to show
that tuple is defined.

Tuples can also be used to make two assignments at the same time:

```{code-cell}
x, y = 10, 20
x
```

```{code-cell}
y
```

This can be used to *swap* to objects within one line. For example

```{code-cell}
x = 1
y = 2
x, y = y, x
x
```

```{code-cell}
y
```

The empty tuple is given by `()`

```{code-cell}
t = ()
len(t)
```

```{code-cell}
type(t)
```

The notation for a tuple containing one value may seem a bit odd at first:

```{code-cell}
t = (42,)
type(t)
```

```{code-cell}
len(t)
```

The extra comma is required to distinguish `(42,)` from `(42)` where in the
latter case the parenthesis would be read as defining operator precedence:
`(42)` simplifies to `42` which is just a number:

```{code-cell}
t = (42)
type(t)
```

This example shows the immutability of a tuple:

```{code-cell}
a = (12, 13, 'dog')
a[0]
```

```{code-cell}
:tags: [raises-exception]

# NBVAL_RAISES_EXCEPTION
a[0] = 1
```

The immutability is the main difference between a tuple and a list (the latter
being mutable). We should use tuples when we don’t want the content to change.

Note that Python functions that return more than one value, return these in
tuples (which makes sense because you don’t want these values be changed).

### Indexing sequences

- Introduction to strings and indexing in [Python tutorial, section
  3.1.2](https://docs.python.org/3/tutorial/introduction.html#strings), the
  relevant section is starting after strings have been introduced.

Individual objects in lists can be accessed by using the index of the object and
square brackets (`[` and `]`):

```{code-cell}
a = ['dog', 'cat', 'mouse']
a[0]
```

```{code-cell}
a[1]
```

```{code-cell}
a[2]
```

Note that Python (like C but unlike Fortran and unlike Matlab) starts counting
indices from zero!

Python provides a handy shortcut to retrieve the last element in a list: for
this one uses the index “-1” where the minus indicates that it is one element
*from the back* of the list. Similarly, the index “-2” will return the 2nd last
element:

```{code-cell}
a = ['dog', 'cat', 'mouse']
a[-1]
```

```{code-cell}
a[-2]
```

If you prefer, you can think of the index `a[-1]` to be a shorthand notation for
`a[len(a) - 1]`.

Remember that strings (like lists) are also a sequence type and can be indexed
in the same way:

```{code-cell}
a = "Hello World!"
a[0]
```

```{code-cell}
a[1]
```

```{code-cell}
a[10]
```

```{code-cell}
a[-1]
```

```{code-cell}
a[-2]
```

### Slicing sequences

- Introduction to strings, indexing and slicing in [Python tutorial, section
  3.1.2](https://docs.python.org/3/tutorial/introduction.html#strings)

*Slicing* of sequences can be used to retrieve more than one element. For
example:

```{code-cell}
a = "Hello World!"
a[0:3]
```

By writing `a[0:3]` we request the first 3 elements starting from element 0. Similarly:

```{code-cell}
a[1:4]
```

```{code-cell}
a[0:2]
```

```{code-cell}
a[0:6]
```

We can use negative indices to refer to the end of the sequence:

```{code-cell}
a[0:-1]
```

It is also possible to leave out the start or the end index and this will return
all elements up to the beginning or the end of the sequence. Here are some
examples to make this clearer:

```{code-cell}
a = "Hello World!"
a[:5]
```

```{code-cell}
a[5:]
```

```{code-cell}
a[-2:]
```

```{code-cell}
a[:]
```

Note that `a[:]` will generate a *copy* of `a`. The use of indices in slicing is
by some people experienced as counter intuitive. If you feel uncomfortable with
slicing, have a look at this quotation from the [Python tutorial (section
3.1.2)](https://docs.python.org/3/tutorial/introduction.html#strings):

> The best way to remember how slices work is to think of the indices as
> pointing between characters, with the left edge of the first character
> numbered 0. Then the right edge of the last character of a string of 5
> characters has index 5, for example:
>
>      +---+---+---+---+---+
>      | H | e | l | l | o |
>      +---+---+---+---+---+
>      0   1   2   3   4   5   <-- use for SLICING
>     -5  -4  -3  -2  -1       <-- use for SLICING
>                                      from the end
>
> The first row of numbers gives the position of the slicing indices 0...5 in
> the string; the second row gives the corresponding negative indices. The slice
> from i to j consists of all characters between the edges labelled i and j,
> respectively.

So the important statement is that for *slicing* we should think of indices
pointing between characters.

For *indexing* it is better to think of the indices referring to characters.
Here is a little graph summarising these rules:

       0   1   2   3   4    <-- use for INDEXING
      -5  -4  -3  -2  -1    <-- use for INDEXING
     +---+---+---+---+---+          from the end
     | H | e | l | l | o |
     +---+---+---+---+---+
     0   1   2   3   4   5  <-- use for SLICING
    -5  -4  -3  -2  -1      <-- use for SLICING
                             from the end

If you are not sure what the right index is, it is always a good technique to
play around with a small example at the Python prompt to test things before or
while you write your program.

### Dictionaries

Dictionaries are also called “associative arrays” and “hash tables”.
Dictionaries are *unordered* sets of *key-value pairs*.

An empty dictionary can be created using curly braces:

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
d = {}
```

Keyword-value pairs can be added like this:

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
d['today'] = "22 deg C"  # 'today' is the keyword
```

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
d['yesterday'] = "19 deg C"
```

`d.keys()` returns a list of all keys:

```{code-cell}
d.keys()
```

We can retrieve values by using the keyword as the index:

```{code-cell}
d['today']
```

Other ways of populating a dictionary if the data is known at creation time are:

```{code-cell}
d2 = {2: 4, 3: 9, 4: 16, 5: 25}
d2
```

```{code-cell}
d3 = dict(a=1, b=2, c=3)
d3
```

The function `dict()` creates an empty dictionary.

Other useful dictionary methods include `values()`, `items()` and `get()`. You
can use `in` to check for the presence of values.

```{code-cell}
d.values()
```

```{code-cell}
d.items()
```

```{code-cell}
d.get('today', 'unknown')
```

```{code-cell}
d.get('tomorrow', 'unknown')
```

```{code-cell}
'today' in d
```

```{code-cell}
'tomorrow' in d
```

The method `get(key,default)` will provide the value for a given `key` if that
key exists, otherwise it will return the `default` object.

Here is a more complex example:

```{code-cell}
# NBVAL_IGNORE_OUTPUT
order = {}  # create empty dictionary

# add orders as they come in
order['Peter'] = "Pint of bitter"
order['Paul'] = "Half pint of Hoegarden"
order['Mary'] = "Gin Tonic"

# deliver order at bar
for person in order.keys():
    print(person, "requests", order[person])
```

Some more technicalities:

- The keyword can be any (immutable) Python object. This includes:
  - numbers
  - strings
  - tuples
- dictionaries are very fast in retrieving values (when given the key)

An other example to demonstrate an advantage of using dictionaries over pairs of
lists:

```{code-cell}
# NBVAL_IGNORE_OUTPUT
dic = {}  # create empty dictionary

dic['Hans'] = "room 1033"  # fill dictionary
dic['Andy C'] = "room 1031"  # "Andy C" is key
dic['Ken'] = "room 1027"  # "room 1027" is value

for key in dic.keys():
    print(key, "works in", dic[key])
```

Without dictionary:

```{code-cell}
people = ['Hans', 'Andy C', 'Ken']
rooms = ['room 1033', 'room 1031', 'room 1027']

# possible inconsistency here since we have two lists
if not len(people) == len(rooms):
    raise RuntimeError("people and rooms differ in length")

for i in range(len(rooms)):
    print(people[i], "works in", rooms[i])
```

## Passing arguments to functions

This section contains some more advanced ideas and makes use of concepts that
are only later introduced in this text. The section may be more easily
accessible at a later stage.

When objects are passed to a function, Python always passes (the value of) the
reference to the object to the function. Effectively this is calling a function
by reference, although one could refer to it as calling by value (of the
reference).

We review argument passing by value and reference before discussing the
situation in Python in more detail.

### Call by value

One might expect that if we pass an object by value to a function, that
modifications of that value inside the function will not affect the object
(because we don’t pass the object itself, but only its value, which is a copy).
Here is an example of this behaviour (in C):

```c
#include <stdio.h>

void pass_by_value(int m) {
  printf("in pass_by_value: received m=%d\n",m);
  m=42;
  printf("in pass_by_value: changed to m=%d\n",m);
}

int main(void) {
  int global_m = 1;
  printf("global_m=%d\n",global_m);
  pass_by_value(global_m);
  printf("global_m=%d\n",global_m);
  return 0;
}
```

together with the corresponding output:

    global_m=1
    in pass_by_value: received m=1
    in pass_by_value: changed to m=42
    global_m=1


The value `1` of the global variable `global_m` is not modified when the
function `pass_by_value` changes its input argument to 42.

### Call by reference

Calling a function by reference, on the other hand, means that the object given
to a function is a reference to the object. This means that the function will
see the same object as in the calling code (because they are referencing the
same object: we can think of the reference as a pointer to the place in memory
where the object is located). Any changes acting on the object inside the
function, will then be visible in the object at the calling level (because the
function does actually operate on the same object, not a copy of it).

Here is one example showing this using pointers in C:

```c
#include <stdio.h>

void pass_by_reference(int *m) {
  printf("in pass_by_reference: received m=%d\n",*m);
  *m=42;
  printf("in pass_by_reference: changed to m=%d\n",*m);
}

int main(void) {
  int global_m = 1;
  printf("global_m=%d\n",global_m);
  pass_by_reference(&global_m);
  printf("global_m=%d\n",global_m);
  return 0;
}
```

together with the corresponding output:

    global_m=1
    in pass_by_reference: received m=1
    in pass_by_reference: changed to m=42
    global_m=42

C++ provides the ability to pass arguments as references by adding an ampersand
in front of the argument name in the function definition:

```cpp
#include <stdio.h>

void pass_by_reference(int &m) {
  printf("in pass_by_reference: received m=%d\n",m);
  m=42;
  printf("in pass_by_reference: changed to m=%d\n",m);
}

int main(void) {
  int global_m = 1;
  printf("global_m=%d\n",global_m);
  pass_by_reference(global_m);
  printf("global_m=%d\n",global_m);
  return 0;
}
```

together with the corresponding output:

    global_m=1
    in pass_by_reference: received m=1
    in pass_by_reference: changed to m=42
    global_m=42

### Argument passing in Python

In Python, objects are passed as the value of a reference (think pointer) to the
object. Depending on the way the reference is used in the function and depending
on the type of object it references, this can result in pass-by-reference
behaviour (where any changes to the object received as a function argument, are
immediately reflected in the calling level).

Here are three examples to discuss this. We start by passing a list to a
function which iterates through all elements in the sequence and doubles the
value of each element:

```{code-cell}
def double_the_values(l):
    print("in double_the_values: l = %s" % l)
    for i in range(len(l)):
        l[i] = l[i] * 2
    print("in double_the_values: changed l to l = %s" % l)


l_global = [0, 1, 2, 3, 10]
print("In main: s=%s" % l_global)
double_the_values(l_global)
print("In main: s=%s" % l_global)
```

The variable `l` is a reference to the list object. The line `l[i] = l[i] * 2`
first evaluates the right-hand side and reads the element with index `i`, then
multiplies this by two. A reference to this new object is then stored in the
list object `l` at position with index `i`. We have thus modified the list
object, that is referenced through `l`.

The reference to the list object does never change: the line `l[i] = l[i] * 2`
changes the elements `l[i]` of the list `l` but never changes the reference `l`
for the list. Thus both the function and calling level are operating on the same
object through the references `l` and `global_l`, respectively.

In contrast, here is an example where do not modify the elements of the list
within the function: which produces this output:

```{code-cell}
def double_the_list(l):
    print("in double_the_list: l = %s" % l)
    l = l + l
    print("in double_the_list: changed l to l = %s" % l)


l_global = "Hello"
print("In main: l=%s" % l_global)
double_the_list(l_global)
print("In main: l=%s" % l_global)
```

What happens here is that during the evaluation of `l = l + l` a new object is
created that holds `l + l`, and that we then bind the name `l` to it. In the
process, we lose the references to the list object `l` that was given to the
function (and thus we do not change the list object given to the function).

Finally, let’s look at which produces this output:

```{code-cell}
def double_the_value(l):
    print("in double_the_value: l = %s" % l)
    l = 2 * l
    print("in double_the_values: changed l to l = %s" % l)


l_global = 42
print("In main: s=%s" % l_global)
double_the_value(l_global)
print("In main: s=%s" % l_global)
```

In this example, we also double the value (from 42 to 84) within the function.
However, when we bind the object 84 to the python name `l` (that is the line `l
= l * 2`) we have created a new object (84), and we bind the new object to `l`.
In the process, we lose the reference to the object 42 within the function. This
does not affect the object 42 itself, nor the reference `l_global` to it.

In summary, Python’s behaviour of passing arguments to a function may appear to
vary (if we view it from the pass by value versus pass by reference point of
view). However, it is always call by value, where the value is a reference to
the object in question, and the behaviour can be explained through the same
reasoning in every case.

### Performance considerations

Call by value function calls require copying of the value before it is passed to
the function. From a performance point of view (both execution time and memory
requirements), this can be an expensive process if the value is large. (Imagine
the value is a `numpy.array` object which could be several Megabytes or
Gigabytes in size.)

One generally prefers call by reference for large data objects as in this case
only a pointer to the data objects is passed, independent of the actual size of
the object, and thus this is generally faster than call-by-value.

Python’s approach of (effectively) calling by reference is thus efficient.
However, we need to be careful that our function do not modify the data they
have been given where this is undesired.

### Inadvertent modification of data

Generally, a function should not modify the data given as input to it.

For example, the following code demonstrates the attempt to determine the
maximum value of a list, and – inadvertently – modifies the list in the process:

```{code-cell}
def mymax(s):  # demonstrating side effect
    if len(s) == 0:
        raise ValueError("mymax() arg is an empty sequence")
    elif len(s) == 1:
        return s[0]
    else:
        for i in range(1, len(s)):
            if s[i] < s[i - 1]:
                s[i] = s[i - 1]
        return s[len(s) - 1]


s = [-45, 3, 6, 2, -1]
print("in main before caling mymax(s): s=%s" % s)
print("mymax(s)=%s" % mymax(s))
print("in main after calling mymax(s): s=%s" % s)
```

The user of the `mymax()` function would not expect that the input argument is
modified when the function executes. We should generally avoid this. There are
several ways to find better solutions to the given problem:

- In this particular case, we could use the Python in-built function `max()` to
  obtain the maximum value of a sequence.
- If we felt we need to stick to storing temporary values inside the list \[this
  is actually not necessary\], we could create a copy of the incoming list `s`
  first, and then proceed with the algorithm (see [below](#Copying-objects) on
  Copying objects).
- Use another algorithm which uses an extra temporary variable rather than
  abusing the list for this. For example:
- We could pass a tuple (instead of a list) to the function: a tuple is
  *immutable* and can thus never be modified (this would result in an exception
  being raised when the function tries to write to elements in the tuple).

### Copying objects

Python provides the `id()` function which returns an integer number that is
unique for each object. (In the current CPython implementation, this is the
memory address.) We can use this to identify whether two objects are the same.

To copy a sequence object (including lists), we can slice it, *i.e.* if `a` is a
list, then `a[:]` will return a copy of `a`. Here is a demonstration:

```{code-cell}
a = list(range(10))
a
```

```{code-cell}
b = a
b[0] = 42
a  # changing b changes a
```

```{code-cell}
# NBVAL_IGNORE_OUTPUT
id(a)
```

```{code-cell}
# NBVAL_IGNORE_OUTPUT
id(b)
```

```{code-cell}
# NBVAL_IGNORE_OUTPUT
c = a[:]
id(c)  # c is a different object
```

```{code-cell}
c[0] = 100
a  # changing c does not affect a
```

Python’s standard library provides the `copy` module, which provides copy
functions that can be used to create copies of objects. We could have used
`import copy; c = copy.deepcopy(a)` instead of `c = a[:]`.

## Equality and Identity/Sameness

A related question concerns the equality of objects.

### Equality

The operators `<`, `>`, `==`, `>=`, `<=`, and `!=` compare the *values* of two
objects. The objects need not have the same type. For example:

```{code-cell}
a = 1.0
b = 1
type(a)
```

```{code-cell}
type(b)
```

```{code-cell}
a == b
```

So the `==` operator checks whether the values of two objects are equal.

### Identity / Sameness

To see check whether two objects `a` and `b` are the same (i.e. `a` and `b` are
references to the same place in memory), we can use the `is` operator (continued
from example above):

```{code-cell}
a is b
```

Of course they are different here, as they are not of the same type.

We can also ask the `id` function which, according to the documentation string
in Python 2.7 “*Returns the identity of an object. This is guaranteed to be
unique among simultaneously existing objects. (Hint: it’s the object’s memory
address.)*”

```{code-cell}
# NBVAL_IGNORE_OUTPUT
id(a)
```

```{code-cell}
# NBVAL_IGNORE_OUTPUT
id(b)
```

which shows that `a` and `b` are stored in different places in memory.

### Example: Equality and identity

We close with an example involving lists:

```{code-cell}
x = [0, 1, 2]
y = x
x == y
```

```{code-cell}
x is y
```

```{code-cell}
# NBVAL_IGNORE_OUTPUT
id(x)
```

```{code-cell}
# NBVAL_IGNORE_OUTPUT
id(y)
```

Here, `x` and `y` are references to the same piece of memory, they are thus
identical and the `is` operator confirms this. The important point to remember
is that line 2 (`y=x`) creates a new reference `y` to the same list object that
`x` is a reference for.

Accordingly, we can change elements of `x`, and `y` will change simultaneously
as both `x` and `y` refer to the same object:

```{code-cell}
x
```

```{code-cell}
y
```

```{code-cell}
x is y
```

```{code-cell}
x[0] = 100
y
```

```{code-cell}
x
```

In contrast, if we use `z=x[:]` (instead of `z=x`) to create a new name `z`,
then the slicing operation `x[:]` will actually create a copy of the list `x`,
and the new reference `z` will point to the copy. The *value* of `x` and `z` is
equal, but `x` and `z` are not the same object (they are not identical):

```{code-cell}
x
```

```{code-cell}
z = x[:]  # create copy of x before assigning to z
z == x  # same value
```

```{code-cell}
z is x  # are not the same object
```

```{code-cell}
# NBVAL_IGNORE_OUTPUT
id(z)  # confirm by looking at ids
```

```{code-cell}
# NBVAL_IGNORE_OUTPUT
id(x)
```

```{code-cell}
x
```

```{code-cell}
z
```

Consequently, we can change `x` without changing `z`, for example (continued)

```{code-cell}
x[0] = 42
x
```

```{code-cell}
z
```
