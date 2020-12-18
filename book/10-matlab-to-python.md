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

# From Matlab to Python

## Important commands

### The for-loop

Matlab:

```octave
for i = 1:10
    disp(i)
end
```

+++

Matlab requires the `end` key-word at the end of the block belonging to the
for-loop.

Python:

```{code-cell} ipython3
for i in range(1,11):
    print(i)
```

Python requires a colon (“:”) at the of the `for`-line. (This is important and
often forgotten when you have programmed in Matlab before.) Python requires the
commands to be executed within the for-loop to be indented.

### The if-then statement

Matlab:

```octave
if a==0
    disp('a is zero')
elseif a<0
    disp('a is negative')
elseif a==42
    disp('a is 42')
else
    disp('a is positive')
end
```

Matlab requires the `end` key-word at the very end of the block belonging to the
for-loop.

Python:

```{code-cell} ipython3
a = -5

if a==0:
    print('a is zero')
elif a<0:
    print('a is negative')
elif a==42:
    print('a is 42')
else:
    print('a is positive')
```

Python requires a colon (“:”) after every condition (i.e. at the of the lines
starting with `if`, `elif`, `else`. Python requires the commands to be executed
within each part of the if-then-else statement to be indented.

### Indexing

Matlab’s indexing of matrices and vectors starts a 1 (similar to Fortran),
whereas Python’s indexing starts at 0 (similar to C).

### Matrices

In Matlab, every object is a matrix. In Python, there is a specialised extension
library called `numpy` (see Sec. \[cha:numer-pyth-numpy\]) which provides the
`array` object which in turns provides the corresponding functionality. Similar
to Matlab, the `numpy` object is actually based on binary libraries and
execution there very fast.

There is a dedicated introduction to numpy for Matlab users available at
<https://numpy.org/doc/stable/user/numpy-for-matlab-users.html>.
