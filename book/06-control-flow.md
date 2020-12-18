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

# Control Flow

## Basics

For a given file with a python program, the python interpreter will start at the
top and then process the file. We demonstrate this with a simple program, for
example:

```{code-cell}
def f(x):
    """function that computes and returns x*x"""
    return x * x


print("Main program starts here")
print("4 * 4 = %s" % f(4))
print("In last line of program -- bye")
```

The basic rule is that commands in a file (or function or any sequence of
commands) is processed from top to bottom. If several commands are given in the
same line (separated by `;`), then these are processed from left to right
(although it is discouraged to have multiple statements per line to maintain
good readability of the code.)

In this example, the interpreter starts at the top (line 1). It finds the `def`
keyword and remembers for the future that the function `f` is defined here. (It
will not yet execute the function body, i.e. line 3 – this only happens when we
call the function.) The interpreter can see from the indentation where the body
of the function stops: the indentation in line 5 is different from that of the
first line in the function body (line2), and thus the function body has ended,
and execution should carry on with that line. (Empty lines do not matter for
this analysis.)

In line 5 the interpreter will print the output `Main program starts here`. Then
line 6 is executed. This contains the expression `f(4)` which will call the
function `f(x)` which is defined in line 1 where `x` will take the value `4`.
\[Actually `x` is a reference to the object `4`.\] The function `f` is then
executed and computes and returns `4*4` in line 3. This value `16` is used in
line 6 to replace `f(4)` and then the string representation `%s` of the object
16 is printed as part of the print command in line 6.

The interpreter then moves on to line 7 before the program ends.

We will now learn about different possibilities to direct this control flow
further.

### Conditionals

The python values `True` and `False` are special inbuilt objects:

```{code-cell}
a = True
print(a)
```

```{code-cell}
type(a)
```

```{code-cell}
b = False
print(b)
```

```{code-cell}
type(b)
```

We can operate with these two logical values using boolean logic, for example
the logical and operation (`and`):

```{code-cell}
True and True  # logical and operation
```

```{code-cell}
True and False
```

```{code-cell}
False and True
```

```{code-cell}
True and True
```

```{code-cell}
c = a and b
print(c)
```

There is also logical or (`or`) and the negation (`not`):

```{code-cell}
True or False
```

```{code-cell}
not True
```

```{code-cell}
not False
```

```{code-cell}
True and not False
```

In computer code, we often need to evaluate some expression that is either true
or false (sometimes called a “predicate”). For example:

```{code-cell}
x = 30  # assign 30 to x
x > 15  # is x greater than 15
```

```{code-cell}
x > 42
```

```{code-cell}
x == 30  # is x the same as 30?
```

```{code-cell}
x == 42
```

```{code-cell}
not x == 42  # is x not the same as 42?
```

```{code-cell}
x != 42  # is x not the same as 42?
```

```{code-cell}
x > 30  # is x greater than 30?
```

```{code-cell}
x >= 30  # is x greater than or equal to 30?
```

## If-then-else

**Further information**

- Introduction to If-then in [Python tutorial, section 4.1](https://docs.python.org/3/tutorial/controlflow.html#if-statements)

The `if` statement allows conditional execution of code, for example:

```{code-cell}
a = 34
if a > 0:
    print("a is positive")
```

The if-statement can also have an `else` branch which is executed if the
condition is wrong:

```{code-cell}
a = 34
if a > 0:
    print("a is positive")
else:
    print("a is non-positive (i.e. negative or zero)")
```

Finally, there is the `elif` (read as “else if”) keyword that allows checking
for several (exclusive) possibilities:

```{code-cell}
a = 17
if a == 0:
    print("a is zero")
elif a < 0:
    print("a is negative")
else:
    print("a is positive")
```

## For loop

Further information:
- Introduction to for-loops in [Python tutorial, section 4.2](https://docs.python.org/3/tutorial/controlflow.html#for-statements)

The `for`-loop allows to iterate over a sequence (this could be a string or a
list, for example). Here is an example:

```{code-cell}
for animal in ['dog', 'cat', 'mouse']:
    print(animal, animal.upper())
```

Together with the `range()` command (03 Data Types Structures, The Range
Command), one can iterate over increasing integers:

```{code-cell}
for i in range(5, 10):
    print(i)
```

## While loop

The `while` keyword allows to repeat an operation while a condition is true.
Suppose we’d like to know for how many years we have to keep 100 pounds on a
savings account to reach 200 pounds simply due to annual payment of interest at
a rate of 5%. Here is a program to compute that this will take 15 years:

```{code-cell}
mymoney = 100  # in GBP
rate = 1.05  # 5% interest
years = 0
while mymoney < 200:  # repeat until 20 pounds reached
    mymoney = mymoney * rate
    years = years + 1
print("We need", years, "years to reach", mymoney, "pounds.")
```

## Relational operators (comparisons) in `if` and `while` statements

The general form of `if` statements and `while` loops is the same: following the
keyword `if` or `while`, there is a *condition* followed by a colon. In the next
line, a new (and thus indented!) block of commands starts that is executed if
the condition is True).

For example, the condition could be equality of two variables `a1` and `a2`
which is expressed as `a1==a2`:

```{code-cell}
a1 = 42
a2 = 42
if a1 == a2:
    print("a1 and a2 are the same")
```

Another example is to test whether `a1` and `a2` are not the same. For this, we
have two possibilities. Option number 1 uses the *inequality operator* `!=`:

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
if a1 != a2:
    print("a1 and a2 are different")
```

Option two uses the keyword `not` in front of the condition:

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
if not a1 == a2:
    print("a1 and a2 are different")
```

Comparisons for “greater” (`>`), “smaller” (`<`) and “greater equal” (`>=`) and
“smaller equal” (`<=`) are straightforward.

Finally, we can use the logical operators “`and`” and “`or`” to combine
conditions:

```{code-cell}
if a > 10 and b > 20:
    print("A is greater than 10 and b is greater than 20")
if a > 10 or b < -5:
    print("Either a is greater than 10, or " "b is smaller than -5, or both.")
```

Use the Python prompt to experiment with these comparisons and logical
expressions. For example:

```{code-cell}
T = -12.5
if T < -20:
    print("very cold")

if T < -10:
    print("quite cold")
```

```{code-cell}
T < -20
```

```{code-cell}
T < -10
```

## Exceptions

TODO: CARRY ON FROM HERE

Even if a statement or expression is syntactically correct, it may cause an
error when an attempt is made to execute it. Errors detected during execution
are called *exceptions* and are not necessarily fatal: exceptions can be
*caught* and dealt with within the program. Most exceptions are not handled by
programs, however, and result in error messages as shown here

```{code-cell}
:tags: [raises-exception]

# NBVAL_RAISES_EXCEPTION
10 * (1 / 0)
```

```{code-cell}
:tags: [raises-exception]

# NBVAL_RAISES_EXCEPTION
4 + spam * 3
```

```{code-cell}
:tags: [raises-exception]

# NBVAL_SKIP
"2" + 2
```

Schematic exception catching with all options

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
try:
    # code body
    pass
except ArithmeticError:
    # what to do if arithmetic error
    pass
except IndexError as the_exception:
    # the_exception refers to the exeption in this block
    pass
except:
    # what to do for ANY other exception
    pass
else:  # optional
    # what to do if no exception raised
    pass

try:
    # code body
    pass
finally:
    # what to do ALWAYS
    pass
```

Starting with Python 2.5, you can use the with statement to simplify the writing
of code for some predefined functions, in particular the `open` function to open
files: see
<https://docs.python.org/3/tutorial/errors.html#predefined-clean-up-actions>.

Example: We try to open a file that does not exist, and Python will raise an
exception of type `IOError` which stands for Input Output Error:

```{code-cell}
:tags: [raises-exception]

# NBVAL_RAISES_EXCEPTION
f = open("filenamethatdoesnotexist", "r")
```

If we were writing an application with a user-interface where the user has to
type or select a filename, we would not want to application to stop if the file
does not exist. Instead, we need to catch this exception and act accordingly
(for example by informing the user that a file with this filename does not exist
and ask whether they want to try another file name). Here is the skeleton for
catching this exception:

```{code-cell}
try:
    f = open('filenamethatdoesnotexist', 'r')
except IOError:
    print('Could not open that file')
```

There is a lot more to be said about exceptions and their use in larger
programs. Start reading [Python Tutorial Chapter 8: Errors and
Exceptions](https://docs.python.org/3/tutorial/errors.html#errors-and-exceptions)
if you are interested.

### Raising Exceptions

Raising exception is also referred to as ’throwing an exception’.

Possibilities of raising an Exception

- `raise OverflowError`
- `raise OverflowError, "Bath is full"` (Old style, now discouraged)
- `raise OverflowError("Bath is full")`
- `e = OverflowError("Bath is full"); raise e`

#### Exception hierarchy

The standard exceptions are organized in an inheritance hierarchy e.g.
OverflowError is a subclass of ArithmeticError (not BathroomError); this can be
seen when looking at `help(’exceptions’)` for example.

You can derive your own exceptions from any of the standard ones. It is good
style to have each module define its own base exception.

### Creating our own exceptions

- You can and should derive your own exceptions from the built-in Exception.
- To see what built-in exceptions exist, look in the module exceptions (try
  `help(’exceptions’)`), or go to
  <https://docs.python.org/3/library/exceptions.html#bltin-exceptions>.

### LBYL vs EAFP

-   LBYL (Look Before You Leap) vs
-   EAFP (Easer to ask forgiveness than permission)

```{code-cell}
---
jupyter:
  outputs_hidden: true
---
numerator = 7
denominator = 0
```

Example for LBYL:

```{code-cell}
if denominator == 0:
    print("Oops")
else:
    print(numerator / denominator)
```

Easier to Ask for Forgiveness than Permission:

```{code-cell}
try:
    print(numerator / denominator)
except ZeroDivisionError:
    print("Oops")
```

The Python documentation says about EAFP:

> Easier to ask for forgiveness than permission. This common Python coding style
> assumes the existence of valid keys or attributes and catches exceptions if
> the assumption proves false. This clean and fast style is characterized by the
> presence of many try and except statements. The technique contrasts with the
> LBYL style common to many other languages such as C.

Source: <https://docs.python.org/3/glossary.html#term-eafp>

The Python documentation says about LBYL:

> Look before you leap. This coding style explicitly tests for pre-conditions
> before making calls or lookups. This style contrasts with the EAFP approach
> and is characterized by the presence of many if statements.
>
> In a multi-threaded environment, the LBYL approach can risk introducing a race
> condition between “the looking” and “the leaping”. For example, the code, if
> key in mapping: return mapping\[key\] can fail if another thread removes key
> from mapping after the test, but before the lookup. This issue can be solved
> with locks or by using the EAFP approach.

Source: <https://docs.python.org/3/glossary.html#term-lbyl>

EAFP is the Pythonic way.
