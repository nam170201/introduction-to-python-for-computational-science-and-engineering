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

# Python shells

## IDLE

IDLE comes with every Python distribution and is a useful tool for everyday
programming. Its editor provides syntax highlighting.

There are two reasons why you might want to use another Python shell, for
example:

- While working with the Python prompt, you like auto-completion of variable
  names, filenames and commands. In that case *IPython* is your tool of choice
  (see below). IPython does not provide an editor, but you can carry on using
  the IDLE editor to edit files, or any other editor you like.

IPython provides a number of nice features for the more experienced Python
programmer, including convenient profiling of code (see <https://ipython.org/>).

Recently, some auto-completion has been added to Idle as well (press tab after
having typed the first few letters of object names and keywords).

## Python (command line)

This is the most basic face of the Python shell. It is very similar to the
Python prompt in IDLE but there are no menus to click on and no facilities to
edit files.

## Interactive Python (IPython)

### IPython console

IPython is an improved version of the Python command line. It is a valuable tool
and worth exploring it’s capabilities (see
<https://ipython.org/ipython-doc/stable/interactive/qtconsole.html>)

You will find the following features very useful:

- auto completion: Suppose you want to type `a = range(10)`. Instead of typing
  all the letters, just type `a = ra` and the press the “Tab” key. Ipython will
  now show all the possible commands (and variable names) that start with `ra`.
  If you type a third letter, here `n` and press “Tab” again, Ipython will auto
  complete and append `ge` automatically. This works also for variable names and
  modules.
- To obtain help on a command, we can use Python’s help command. For example:
  `help(range)`. Ipython provides a shortcut. To achieve the same, it is
  sufficient to just type the command followed by a question mark: `range?`
- You can relatively easily navigate directories on your computer. For example,
  - `!dir` lists the content of the current directory (same as `ls`)
  - `pwd` shows the current working directory
  - `cd` allows to change directories
- In general, using an exclamation mark before the command will pass the command
  to the shell (not to the Python interpreter).
- You can execute Python programs from ipython using `%run`. Suppose you have a
  file `hello.py` in the current directory. You can then execute it by typing:
  `%run hello` Note that this differs from executing a python program in IDLE:
  IDLE restarts the Python interpreter session and thus deletes all existing
  objects before the execution starts. This is not the case with the `run`
  command in ipython (and neither when executing chunks of Python code from
  Emacs using the Emacs Python mode). In particular this can be very useful if
  one needs to setup a few objects which are needed to test the code one is
  working on. Using ipython’s `run` or Emacs instead of IDLE allows to keep
  these objects in the interpreter session and to only update the
  function/classes/... etc that are being developed.
- allows multi-line editing of command history
- provides on-the-fly syntax highlighting
- displays doc-strings on-the-fly
- can inline matplotlib figures (activate mode with if started with `%matplotlib
  inline`)
- `%load` loads file from disk or form URL for editing
- `%timeit` measures execution time for a given statement
- …and a lot more.
- Read more at <https://ipython.org/ipython-doc/dev/interactive/qtconsole.html>

If you have access to this shell, you may want to consider it as your default Python prompt.

### Jupyter Notebook

The Jupyter Notebook (formerly IPython Notebook) allows you to execute, store,
load, re-execute a sequence of Python commands, and to include explanatory text,
images and other media in between.

This is a recent and exciting development that has the potential to develop into
a tool of great significance, for example for

- documenting calculations and data processing
- support learning and teaching of
  - Python itself
  - statistical methods
  - general data post-processing
  - …
- documentation new code
- automatic regression testing by re-running ipython notebook and comparing
  stored output with computed output

Further reading:
- Jupyter Notebook (<https://jupyter-notebook.readthedocs.io/en/latest/>).
- IPython (<https://ipython.org>).

## Spyder

Spyder is the Scientific PYthon Development EnviRonment: a powerful interactive
development environment for the Python language with advanced editing,
interactive testing, debugging and introspection features and a numerical
computing environment thanks to the support of IPython (enhanced interactive
Python interpreter) and popular Python libraries such as NumPy (linear algebra),
SciPy (signal and image processing) or matplotlib (interactive 2D/3D plotting).
See <https://www.spyder-ide.org/> for more.

Some important features of Spyder:

- Within Spyder, the IPython console is the default Python interpreter, and
- code in the editor can be fully or partially be executed in this buffer
- The editor supports automatic checking for Python erros using pyflakes, and
- the editor warns (if desired) if the code formatting deviates from the PEP8
  style guide
- The Ipython Debugger can be activated, and
- a profiler is provided
- An object explorer shows documentation for functions, methods etc on the fly
  and a
- variable explorer displays names, size and values for numerical variables

Spyder is currently (as of 2014) on the way to develop into a powerful and
robust multi-platform integrated environment for Python development, with
particular emphasis on Python for scientific computing and engineering.

## Editors

All major editors that are used for programming, provide Python modes (such as
Emacs, Vim, Sublime Text), some Integrated Development Environments (IDEs) come
with their own editor (Spyder, Eclipse). Which of these is best, is partly a
matter of choice.

For beginners, Spyder seems a sensible choice as it provides an IDE, allows
execution of chunks of code in an interpreter session and is easy to pick up.
