# Numpy

## Useful functions

`numpy.reshape(a, newshape, order='C')` : Give new shape to an array.

args:

`a` : the array

`newshape`: int or tuple of int indicating the shape. Can use `-1` for a value as inference.

Details see https://docs.scipy.org/doc/numpy/reference/generated/numpy.reshape.html.

---

To square a list elementwise:

`numpy.square(x)`

---

To subtract lists elementwise:

`a - b`

---

To sum all elements:

`numpy.sum(x)`

Note that `sum` can also be used to sum in a certain dimension. See https://docs.scipy.org/doc/numpy/reference/generated/numpy.sum.html.

---

To find mode of a list...

`np.argmax(np.bincount(x))`

---

Split array into folds (k-fold verification!)

https://docs.scipy.org/doc/numpy/reference/generated/numpy.array_split.html

---

