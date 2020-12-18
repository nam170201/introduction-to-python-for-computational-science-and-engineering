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

# Where to go from here?

Learning a programming language is the first step towards becoming a computationalists who advances science and engineering through computational modelling and simulation.

We list some additional skills that can be very beneficial for day-to-day computational science work, but is of course not exhaustive.

## Advanced programming

This text has put emphasis on providing a robust foundation in terms of programming, covering control flow, data structures and elements from function and procedural programming. We have not touch Object Orientation in great detail, nor have we discussed some of Python’s more advanced features such as iterators, and decorators, for example.

## Compiled programming language

When performance starts to be the highest priority, we may need to use compiled code, and likely embed this in a Python code to carry out the computational that are the performance bottle neck.

Fortran, C and C++ are sensible choices here; maybe Julia in the near future.

We also need to learn how to integrate the compiled code with Python using tools such as Cython, Boost, Ctypes and Swig.

## Testing

Good coding is supported by a range of unit and system tests that can be run routinely to check that the code works correctly. Tools such as doctest, nose and pytest are invaluable, and we should learn at least how to use pytest (or nose).

## Simulation models

A number of standard simulation tools such as Monte Carlo, Molecular Dynamics, lattice based models, agents, finite difference and finite element models are commonly used to solve particular simulation challenges – it is useful to have at least a broad overview of these.

## Software engineering for research codes

Research codes bring particular challenges: the requirements may change during the run time of the project, we need great flexibility yet reproducibility. A number of techniques are available to support effectively.

## Data and visualisation

Dealing with large amounts of data, processing and visualising it can be a challenge. Fundamental knowledge of database design, 3d visualisation and modern data processing tools such as the Pandas Python package help with this.

## Version control

Using a version control tool, such as git or mercurial, should be a standard approach and improves code writing effectiveness significantly, helps with working in teams, and - maybe most importantly - supports reproducibility of computational results.

## Parallel execution

Parallel execution of code is a way to make it run orders of magnitude faster. This could be using MPI for inter-node communication or OpenMP for intra-node parallelisation or a hybrid mode bringing both together.

The recent rise of GPU computing provides yet another avenue of parallelisation, and so do the many-core chips such as the Intel Phi.

### Acknowledgements

Big thanks go to

-   Marc Molinari for carefully proof reading this manuscript around 2007.

-   Neil O’Brien for contributing to the SymPy section.

-   Jacek Generowicz for introducing me to Python in the last millennium, and for kindly sharing countless ideas from his Python course.

-   EPSRC (GR/T09156/01 and EP/G03690X/1) and the European Union (OpenDreamKit Horizon 2020 European Research Infrastructures project, #676541) for support.

-   Students and other readers who have provided feedback and pointed out typos and errors etc.

-   Thomas Kluyver who helped to translate the Python 2 LaTeX based document into Python 3 Jupyter Notebooks and provided the machinery to create html and pdf versions. automatically (via his [bookbook package](https://github.com/takluyver/bookbook)).

-   Robert Rosca who helped to create html and pdf files after using [jupyterbook](https://jupyterbook.org) was released (2020).

[1] the vertical line is to show the division between the original components only; mathematically, the augmented matrix behaves like any other 2 × 3 matrix, and we code it in SymPy as we would any other.

[2] from the <span>`help(preview)`</span> documentation: “Currently this depends on pexpect, which is not available for windows.”

[3] The exact value for the upper limit is availabe in `sys.maxint`.

[4] We add for completeness, that a C-program (or C++ of Fortran) that executes the same loop will be about 100 times faster than the python float loop, and thus about 100\*200 = 20000 faster than the symbolic loop.

[5] In this text, we usually import `numpy` under the name `N` like this: `import numpy as N`. If you don’t have `numpy` on your machine, you can substitute this line by `import Numeric as N` or `import numarray as N`.

[6] Historical note: this has changed from scipy version 0.7 to 0.8. Before 0.8, the return value was a float if a one-dimensional problem was to solve.
