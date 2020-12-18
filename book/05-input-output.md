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

# Input and Output

In this section, we describe printing, which includes the use of the `print`
function, the old-style `%` format specifiers  and the new style `{}` format
specifiers.

## Printing to standard output (normally the screen)

The `print` function is the most commonly used command to print information to
the “standard output device” which is normally the screen.

There are two modes to use print.

### Simple print

The easiest way to use the print command is to list the variables to be printed,
separated by comma. Here are a few examples:

```{code-cell}
a = 10
b = "test text"
print(a)
```

```{code-cell}
print(b)
```

```{code-cell}
print(a, b)
```

```{code-cell}
print("The answer is", a)
```

```{code-cell}
print("The answer is", a, "and the string contains", b)
```

```{code-cell}
print("The answer is", a, "and the string reads", b)
```

Python adds a space between every object that is being printed.

Python prints a new line after every print call. To suppress that, use the
`end=` parameter:

```{code-cell}
print("Printing in line one", end="")
print("...still printing in line one.")
```

### Formatted printing

The more sophisticated way of formatting output uses a syntax very similar to
Matlab’s `fprintf` (and therefore also similar to C’s `printf`).

The overall structure is that there is a string containing format specifiers,
followed by a percentage sign and a tuple that contains the variables to be
printed in place of the format specifiers.

```{code-cell}
print('a = %d b = %d' % (10, 20))
```

A string can contain format identifiers (such as `%f` to format as a float, `%d`
to format as an integer, and `%s` to format as a string):

```{code-cell}
from math import pi

print("Pi = %5.2f" % pi)
```

```{code-cell}
print("Pi = %10.3f" % pi)
```

```{code-cell}
print("Pi = %10.8f" % pi)
```

```{code-cell}
print("Pi = %d" % pi)
```

The format specifier of type `%W.Df` means that a Float should be printed with a
total Width of `W` characters and `D` digits behind the Decimal point. (This is
identical to Matlab and C, for example.)

To print more than one object, provide multiple format specifiers and list
several objects in the tuple:

```{code-cell}
print("Pi = %f, 142*pi = %f and pi^2 = %f." % (pi, 142 * pi, pi ** 2))
```

Note that the conversion of a format specifier and a tuple of variables into
string does not rely on the `print` command:

```{code-cell}
from math import pi

"pi = %f" % pi
```

This means that we can convert objects into strings wherever we need, and we can
decide to print the strings later – there is no need to couple the formatting
closely to the code that does the printing.

Overview of commonly used format specifiers using the astronomical unit as an
example:

```{code-cell}
AU = 149597870700  # astronomical unit [m]
"%f" % AU  # line 1 in table
```

| specifier         |         style         |  Example output for AU|
|:------------------|:---------------------:|----------------------:|
| `%f` |     floating point    |  `149597870700.000000`|
| `%e` |  exponential notation |         `1.495979e+11`|
| `%g` |  shorter of %e or %f  |          `1.49598e+11`|
| `%d` |        integer        |         `149597870700`|
| `%s` |  `str()` |         `149597870700`|
| `%r` | `repr()` |        `149597870700L`|

### `str` and `__str__`

All objects in Python should provide a method `__str__` which returns a nice
string representation of the object. This method `a.__str__()` is called when we
apply the `str` function to object `a`:

```{code-cell}
a = 3.14
a.__str__()
```

```{code-cell}
str(a)
```

The `str` function is extremely convenient as it allows us to print more
complicated objects, such as:

```{code-cell}
b = [3, 4.2, ['apple', 'banana'], (0, 1)]
str(b)
```

The way Python prints this is that it uses the `__str__` method of the list
object. This will print the opening square bracket `[` and then call the
`__str__` method of the first object, i.e. the integer 3. This will produce `3`.
Then the list object’s `__str__` method prints the comma `,` and moves on to
call the `__str__` method of the next element in the list (i.e. `4.2`) to print
itself. This way any composite object can be represented as a string by asking
the objects it holds to convert themselves to strings.

The string method of object `x` is called implicitly, when we

- use the “%s” format specifier to print `x`
- pass the object `x` directly to the print command:

```{code-cell}
print(b)
```

```{code-cell}
print("%s" % b)
```

### `repr` and `__repr__`

A second function, `repr`, should convert a given object into a string
presentation *so that this string can be used to re-created the object using the
`eval` function*. The `repr` function will generally provide a more detailed
string than `str`. Applying `repr` to the object `x` will attempt to call
`x.__repr__()`.

```{code-cell}
from math import pi as a1

str(a1)
```

```{code-cell}
repr(a1)
```

```{code-cell}
number_as_string = repr(a1)
a2 = eval(number_as_string)  # evaluate string
a2
```

```{code-cell}
a2 - a1  # -> repr is exact representation
```

```{code-cell}
a1 - eval(repr(a1))
```

```{code-cell}
a1 - eval(str(a1))  # -> str has lost a few digits
```

We can convert an object to its `str()` or `repr` presentation using the format
specifiers `%s` and `%r`, respectively.

```{code-cell}
import math

"%s" % math.pi
```

```{code-cell}
"%r" % math.pi
```

### New-style string formatting

A new system of built-in formatting allows more flexibility for complex cases,
at the cost of being a bit longer.

Basic ideas in examples:

```{code-cell}
"{} needs {} pints".format("Peter", 4)  # insert values in order
```

```{code-cell}
"{0} needs {1} pints".format("Peter", 4)  # index which element
```

```{code-cell}
"{1} needs {0} pints".format("Peter", 4)
```

```{code-cell}
"{name} needs {number} pints".format(  # reference element to
    name="Peter", number=4  # print by name
)
```

```{code-cell}
"Pi is approximately {:f}.".format(
    math.pi
)  # can use old-style format options for float
```

```{code-cell}
"Pi is approximately {:.2f}.".format(math.pi)  # and precision
```

```{code-cell}
"Pi is approximately {:6.2f}.".format(math.pi)  # and width
```

This is a powerful and elegant way of string formatting, which is gradually being used more.

**Further information**

- Examples <https://docs.python.org/3/library/string.html#format-examples>
- [Python Enhancement Proposal 3101](https://www.python.org/dev/peps/pep-3101/)
- [Python library String Formatting Operations](https://docs.python.org/3/library/string.html#formatstrings)
- [Introduction to Fancier Output Formatting, Python tutorial, section 7.1](https://docs.python.org/3/tutorial/inputoutput.html)

+++

### Changes from Python 2 to Python 3: `print`

One (maybe the most obvious) change going from Python 2 to Python 3 is that the
`print` command loses its special status. In Python 2, we could print “Hello
World” using:

```python
print "Hello world"  # valid in Python 2.x
```

Effectively, we call the function `print` with the argument `Hello World`. All
other functions in Python are called such that the argument is enclosed in
parentheses, i.e.

```{code-cell}
print("Hello World")  # valid in Python 3.x
```

This is the new convention *required* in Python 3 (and *allowed* for recent
version of Python 2.x.)

Everything we have learned about formatting strings using the percentage
operator still works the same way:

```{code-cell}
import math

a = math.pi
"my pi = %f" % a  # string formatting
```

```{code-cell}
print("my pi = %f" % a)  # valid print in 2.7 and 3.x
```

```{code-cell}
"Short pi = %.2f, longer pi = %.12f." % (a, a)
```

```{code-cell}
print("Short pi = %.2f, longer pi = %.12f." % (a, a))
```

```{code-cell}
print("Short pi = %.2f, longer pi = %.12f." % (a, a))
```

```{code-cell}
# 1. Write a file
out_file = open("test.txt", "w")  #'w' stands for Writing
out_file.write(
    "Writing text to file. This is the first line.\n" + "And the second line."
)
out_file.close()  # close the file

# 2. Read a file
in_file = open("test.txt", "r")  #'r' stands for Reading
text = in_file.read()  # read complete file into
# string variable text
in_file.close()  # close the file

# 3. Display data
print(text)
```

## Reading and writing files

Here is a program that

1. writes some text to a file with name `test.txt`,
2. and then reads the text again and
3. prints it to the screen.

The data stored in the file `test.txt` is:

```
Writing text to file. This is the first line.
And the second line.
```

In more detail, you have opened a file with the `open` command, and assigned
this open file object to the variable `out_file`. We have then written data to
the file using the `out_file.write` method. Note that in the example above, we
have given a string to the `write` method. We can, of course, use all the
formatting that we have discussed before—see [formatted
printing](#Formatted-printing) and [new style
formatting](#New-style-string-formatting). For example, to write this file with
name table `table.txt` we can use this Python program It is good practice to
`close()` files when we have finished reading and writing. If a Python program
is left in a controlled way (i.e. not through a power cut or an unlikely bug
deep in the Python language or the operating system) then it will close all open
files as soon as the file objects are destroyed. However, closing them actively
as soon as possible is better style.

### File reading examples

We use a file named `myfile.txt` containing the following 3 lines of text for
the examples below:

    This is the first line.
    This is the second line.
    This is a third and last line.

```{code-cell}
f = open("myfile.txt", "w")
f.write(
    "This is the first line.\n"
    "This is the second line.\n"
    "This is a third and last line."
)
f.close()
```

#### `fileobject.read()`

The `fileobject.read()` method reads the whole file, and returns it as one
string (including new line characters).

```{code-cell}
f = open("myfile.txt", "r")
f.read()
```

```{code-cell}
f.close()
```

#### fileobject.readlines()

The `fileobject.readlines()` method returns a list of strings, where each
element of the list corresponds to one line in the string:

```{code-cell}
f = open("myfile.txt", "r")
f.readlines()
```

```{code-cell}
f.close()
```

This is often used to iterate over the lines, and to do something with each
line. For example:

```{code-cell}
f = open("myfile.txt", "r")
for line in f.readlines():
    print("%d characters" % len(line))
f.close()
```

Note that this will read the complete file into a list of strings when the
`readlines()` method is called. This is no problem if we know that the file is
small and will fit into the machine’s memory.

If so, we can also close the file before we process the data, i.e.:

```{code-cell}
f = open("myfile.txt", "r")
lines = f.readlines()
f.close()
for line in lines:
    print("%d characters" % len(line))
```

#### Iterating over lines (file object)

There is a neater possibility to read a file line by line which (i) will only
read one line at a time (and is thus suitable for large files as well) and (ii)
results in more compact code:

```{code-cell}
f = open("myfile.txt", "r")
for line in f:
    print("%d characters" % len(line))
f.close()
```

Here, the file handler `f` acts as in iterator and will return the next line in
every subsequent iteration of the for-loop until the end of the file is reached
(and then the for-loop is terminated).

## Further reading

[Methods of File objects, Tutorial, Section 7.2.1](https://docs.python.org/3/tutorial/inputoutput.html#methods-of-file-objects)
