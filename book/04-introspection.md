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

# Introspection

A Python code can ask and answer questions about itself and the objects it is manipulating.

## dir

`dir()` is a built-in function which returns a list of all the names belonging to some namespace.

-   If no arguments are passed to dir (i.e. `dir()`), it inspects the namespace in which it was called.

-   If `dir` is given an argument (i.e. `dir(<object>)`, then it inspects the namespace of the object which it was passed.

For example:

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
apples = ['Cox', 'Braeburn', 'Jazz']
dir(apples)
```

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
dir()
```

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
name = "Peter"
dir(name)
```

### Magic names

You will find many names which both start and end with a double underscore (e.g. <span>`__name__`</span>). These are called magic names. Functions with magic names provide the implementation of particular python functionality.

For example, the application of the <span>`str`</span> to an object <span>`a`</span>, i.e. <span>`str(a)`</span>, will – internally – result in the method <span>`a.__str__()`</span> being called. This method <span>`__str__`</span> generally needs to return a string. The idea is that the <span>`__str__()`</span> method should be defined for all objects (including those that derive from new classes that a programmer may create) so that all objects (independent of their type or class) can be printed using the <span>`str()`</span> function. The actual conversion of some object <span>`x`</span> to the string is then done via the object specific method <span>`x.__str__()`</span>.

We can demonstrate this by creating a class <span>`my_int`</span> which inherits from the Python’s integer base class, and overrides the <span>`__str__`</span> method. (It requires more Python knowledge than provided up to this point in the text to be able to understand this example.)

```{code-cell} ipython3
class my_int(int):
    """Inherited from int"""
    def __str__(self):
        """Tailored str representation of my int"""
        return "my_int: %s" % (int.__str__(self))

a = my_int(3)
b = int(4)            # equivalent to b = 4
print("a * b = ", a * b)
print("Type a = ", type(a), "str(a) = ", str(a))
print("Type b = ", type(b), "str(b) = ", str(b))
```

**Further Reading**

See → [Python documentation, Data Model](https://docs.python.org/3/reference/datamodel.html)

## type

The `type(<object>)`command returns the type of an object:

```{code-cell} ipython3
type(1)
```

```{code-cell} ipython3
type(1.0)
```

```{code-cell} ipython3
type("Python")
```

```{code-cell} ipython3
import math
type(math)
```

```{code-cell} ipython3
type(math.sin)
```

## isinstance

`isinstance(<object>, <typespec>)` returns True if the given object is an instance of the given type, or any of its superclasses. Use `help(isinstance)` for the full syntax.

```{code-cell} ipython3
isinstance(2,int)
```

```{code-cell} ipython3
isinstance(2.,int)
```

```{code-cell} ipython3
isinstance(a,int)    # a is an instance of my_int
```

```{code-cell} ipython3
type(a)
```

## help

-   The `help(<object>)` function will report the docstring (magic attribute with name `__doc__`) of the object that it is given, sometimes complemented with additional information. In the case of functions, `help` will also show the list of arguments that the function accepts (but it cannot provide the return value).

-   `help()` starts an interactive help environment.

-   It is common to use the `help` command a lot to remind oneself of the syntax and semantic of commands.

<!-- -->

```{code-cell} ipython3
help(isinstance)
```

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
import math
help(math.sin)
```

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
help(math)
```

The `help` function needs to be given the name of an object (which must exist in the current name space). For example `help(math.sqrt)` will not work if the `math` module has not been imported before.

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
help(math.sqrt)
```

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
import math
help(math.sqrt)
```

Instead of importing the module, we could also have given the *string* of `math.sqrt` to the help function, i.e.:

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
help('math.sqrt')
```

`help` is a function which gives information about the object which is passed as its argument. Most things in Python (classes, functions, modules, etc.) are objects, and therefore can be passed to help. There are, however, some things on which you might like to ask for help, which are not existing Python objects. In such cases it is often possible to pass a string containing the name of the thing or concept to help, for example

-   `help(’modules’)` will generate a list of all modules which can be imported into the current interpreter. Note that help(modules) (note absence of quotes) will result in a NameError (unless you are unlucky enough to have a variable called modules floating around, in which case you will get help on whatever that variable happens to refer to.)

-   `help(’some_module’)`, where some\_module is a module which has not been imported yet (and therefore isn’t an object yet), will give you that module’s help information.

-   `help(’some_keyword’)`: For example `and`, `if` or `print` (*i.e.* `help(’and’)`, `help(’if’)` and `help(’print’)`). These are special words recognized by Python: they are not objects and thus cannot be passed as arguments to help. Passing the name of the keyword as a string to help works, but only if you have Python’s HTML documentation installed, and the interpreter has been made aware of its location by setting the environment variable PYTHONDOCS.

## Docstrings

The command `help(<object>)` accesses the documentation strings of objects.

Any literal string appearing as the first item in the definition of a class, function, method or module, is taken to be its *docstring*.

`help` includes the docstring in the information it displays about the object.

In addition to the docstring it may display some other information, for example, in the case of functions, it displays the function’s signature.

The docstring is stored in the object’s `__doc__` attribute.

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
help(math.sin)
```

```{code-cell} ipython3
# NBVAL_IGNORE_OUTPUT
print(math.sin.__doc__)
```

For user-defined functions, classes, types, modules, …), one should always provide a docstring.

Documenting a user-provided function:

```{code-cell} ipython3
def power2and3(x):
    """Returns the tuple (x**2, x**3)"""
    return x**2 ,x**3

power2and3(2)
```

```{code-cell} ipython3
power2and3(4.5)
```

```{code-cell} ipython3
power2and3(0+1j)
```

```{code-cell} ipython3
help(power2and3)
```

```{code-cell} ipython3
print(power2and3.__doc__)
```
