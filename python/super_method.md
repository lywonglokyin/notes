# `super()`

`super()` in python has some mysterious use, and this note will talk about how it works.

## Uses

1. Like all other language, the inheritance call!

2.

## Simple inheritance call

The most practical use of `super()` is to call methods of a parent class. E.g.

```python
class LoggingDict(dict):
    def __setitem__(self, key, value):
        logging.info(f"Setting {key} to {value}.")
        super().__setitem__(key, value)
```

This class would have the same capabilities of its parent, `dict`, but it extends the `__setitem__` method to makke log entries whenever a key is updated.

Benefit:

1. No need to explicitly mention the parent class (e.g. `dict.__setitem__`). Such that it is easier to refactor (?).

2. The specific parent class to select depends on the class calling `super()` AND also the inheritance tree of this class. (this seems more like a troublesome).

## `super()` rule

As mentioned, which class does `super()` refers to depends on:

1. Which class is calling `super()`

2. What is the inhertiance tree of that class.

More specifically, it depends on the Method Resolution Order (MRO) of the class. E.g.:

```python
class LoggingOD(LoggingDict, collections.OrderedDict):
    pass
```

```python
>>> pprint(LogingOD.__mro__)  # super() is looked for at this order.
(<class '__main__.LoggingOD'>,
 <class '__main__.LoggingDict'>,
 <class 'collections.OrderedDict'>,
 <class 'dict'>,
 <class 'object'>)
```

Thus, in this case, LoggingOD's `__setitem__()` looks at `LoggingDict`, which logs the input and calls `super()`, which then `OrderedDict.__setitem__()` is called.


## A more practical use: Parameter checking

By using this hierachy, we can implement parameter checking in object creation. E.g.

```python
class Shape:
    def __init__(self, shapename, **kwds):
        self.shapename = shapename
        super().__init__(**kwds)

class ColoredShape(Shape):
    def __init__(self, color, **kwds):
        self.color = color
        super().__init__(**kwds)
```

At each level, the class strip off the parameter it need, and passes the remaining ones to `super()`. When it reaches `Object`, `**kwds` should be empty.

https://rhettinger.wordpress.com/2011/05/26/super-considered-super/

