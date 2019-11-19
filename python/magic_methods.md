# Magic methods

Basically every method surrounded by double underscores (e.g. `__init__`) are magic methods.

## IPython magic functions

For IPython, there are two types of magic functions: 1) Line magics 2) Cell magics.

### Line magics

Line magics are prefixed with the `%` character and works like OS command-line calls, i.e., the rest of the line is get as argument. It can return results and be used in the right hand side of argument

### Cell magics

Cell magics are prefixed with a double `%%`, and they are functions that get as an argument not only to the rest of the line, but also line below it.

e.g.

```ipython
[1]: %timeit range(1000)
100000 loops, best of 3: 7.76 us per loop

[2]: %%timeit x = range(10000)
...: max(x)
...:
1000 loops, best of 3: 223 us per loop
```