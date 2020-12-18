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

# Common tasks

Here we provide a selection of small example programs addressing some common
tasks and just providing some more Python code that can be read if seeking
inspiration how to address a given problem.

## Many ways to compute a series

As an example, we compute the sum of odd numbers in different ways.

```{code-cell}
def compute_sum1(n):
    """computes and returns the sum of 2,4,6, ..., m
    where m is the largest even number smaller than n.

    For example, with n = 7, we compute 0+2+4+6 = 12.

    This implementation uses a variable 'mysum' that is
    increased in every iteration of the for-loop."""

    mysum = 0
    for i in range(0, n, 2):
        mysum = mysum + i
    return mysum


def compute_sum2(n):
    """computes and returns ...

    This implementation uses a while-loop:
    """

    counter = 0
    mysum = 0
    while counter < n:
        mysum = mysum + counter
        counter = counter + 2

    return mysum


def compute_sum3(n, startfrom=0):
    """computes and returns ...

    This is a recursive implementation:"""

    if n <= startfrom:
        return 0
    else:
        return startfrom + compute_sum3(n, startfrom + 2)


def compute_sum4a(n):
    """A functional approach ... this seems to be
    the shortest and most concise code.
    """
    return sum(range(0, n, 2))


from functools import reduce


def compute_sum4b(n):
    """A functional approach ... not making use of 'sum' which
    happens to exist and is of course convenient here.
    """
    return reduce(lambda a, b: a + b, range(0, n, 2))


def compute_sum4c(n):
    """A functional approach ... a bit faster than compute_sum4b
    as we avoid using lambda.
    """
    import operator

    return reduce(operator.__add__, range(0, n, 2))


def compute_sum4d(n):
    """Using list comprehension."""
    return sum([k for k in range(0, n, 2)])


def compute_sum4e(n):
    """Using another variation of list comprehension."""
    return sum([k for k in range(0, n) if k % 2 == 0])


def compute_sum5(n):
    """Using numerical python (numpy). This is very fast
    (but would only pay off if n >> 10)."""
    import numpy

    return numpy.sum(2 * numpy.arange(0, (n + 1) // 2))


def test_consistency():
    """Check that all compute_sum?? functions in this file produce
    the same answer for all n>=2 and <N.
    """

    def check_one_n(n):
        """Compare the output of compute_sum1 with all other functions
        for a given n>=2. Raise AssertionError if outputs disagree."""
        funcs = [
            compute_sum1,
            compute_sum2,
            compute_sum3,
            compute_sum4a,
            compute_sum4b,
            compute_sum4c,
            compute_sum4d,
            compute_sum4e,
            compute_sum5,
        ]
        ans1 = compute_sum1(n)
        for f in funcs[1:]:
            assert ans1 == f(n), "%s(n)=%d not the same as %s(n)=%d " % (
                funcs[0],
                funcs[0](n),
                f,
                f(n),
            )

    # main testing loop in test_consistency function
    for n in range(2, 1000):
        check_one_n(n)


if __name__ == "__main__":
    m = 7
    correct_result = 12
    thisresult = compute_sum1(m)
    print("this result is {}, expected to be {}".format(thisresult, correct_result))
    # compare with correct result
    assert thisresult == correct_result
    # also check all other methods
    assert compute_sum2(m) == correct_result
    assert compute_sum3(m) == correct_result
    assert compute_sum4a(m) == correct_result
    assert compute_sum4b(m) == correct_result
    assert compute_sum4c(m) == correct_result
    assert compute_sum4d(m) == correct_result
    assert compute_sum4e(m) == correct_result
    assert compute_sum5(m) == correct_result

    # a more systematic check for many values
    test_consistency()
```

All the different implementations shown above compute the same result. There are
a number of things to be learned from this:

- There are a large (probably an infinite) number of solutions for one given
  problem. (This means that writing programs is a task that requires
  creativity!)
- These may achieve the same ’result’ (in this case computation of a number).
- Different solutions may have different characteristics. They might:
  - be faster or slower
  - use less or more memory
  - are easier or more difficult to understand (when reading the source code)
  - can be considered more or less elegant.

+++

## Sorting

Suppose we need to sort a list of 2-tuples of user-ids and names, i.e.

```{code-cell}
mylist = [
    (
        "fangohr",
        "Hans Fangohr",
    ),
    ("admin", "The Administrator"),
    ("guest", "The Guest"),
]
```

which we want to sort in increasing order of user-ids. If there are two or more
identical user-ids, they should be ordered by the order of the names associated
with these user-ids. This behaviour is just the default behaviour of sort (which
goes back to how to sequences are compared).

```{code-cell}
stuff = mylist  # collect your data
stuff.sort()  # sort the data in place
print(stuff)  # inspect the sorted data
```

Sequences are compared by initially comparing the first elements only. If they
differ, then a decision is reached on the basis of those elements only. If the
elements are equal, only then are the next elements in the sequence compared...
and so on, until a difference is found, or we run out of elements. For example:

```{code-cell}
(2, 0) > (1, 0)
```

```{code-cell}
(2, 1) > (1, 3)
```

```{code-cell}
(2, 1) > (2, 1)
```

```{code-cell}
(2, 2) > (2, 1)
```

It is also possible to do:

```{code-cell}
stuff = sorted(stuff)
```

Where the list is not particularly large, it is generally advisable to use the
`sorted` function (which *returns a sorted copy* of the list) over the `sort`
method of a list (which changes the list into sorted order of elements, and
returns None).

However, what if the data we have is stored such that in each tuple in the list,
the name comes first, followed by the id, i.e.:

```{code-cell}
mylist2 = [
    ("Hans Fangohr", "fangohr"),
    ("The Administrator", "admin"),
    ("The Guest", "guest"),
]
```

We want to sort with the id as the primary key. The first approach to do this is
to change the order of `mylist2` to that of `mylist`, and use `sort` as shown
above.

The second, neater approach relies on being able to decipher the cryptic help
for the sorted function. `list.sort()` has the same options, but its help is
less helpful.

```{code-cell}
# NBVAL_IGNORE_OUTPUT
help(sorted)
```

You should notice that `sorted` and `list.sort` have two keyword parameters. The
first of these is called key. You can use this to supply a *key function*, which
will be used to transform the items for sort to compare.

Let’s illustrate this in the context of our exercise, by assuming that we have
stored a list of pairs like this

    pair = name, id

(i.e. as in `mylist2`) and that we want to sort according to id and ignore name.
We can achieve this by writing a function that retrieves only the second element
of the pair it receives:

```{code-cell}
def my_key(pair):
    return pair[1]
```

```{code-cell}
mylist2.sort(key=my_key)
```

This also works with an anonymous function:

```{code-cell}
mylist2.sort(key=lambda p: p[1])
```

### Efficiency

The `key` function will be called exactly once for every element in the list.
This is much more efficient than calling a function on every *comparison* (which
was how you customised sorting in older versions of Python). But if you have a
large list to sort, the overhead of calling a Python function (which is
relatively large compared to the C function overhead) might be noticeable.

If efficiency is really important (and you have proven that a significant
proportion of time is spent in these functions) then you have the option of
re-coding them in C (or another low-level language).
