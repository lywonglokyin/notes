# Python annotations and function decorators

1. Parameter and return value annotation

2. Function decorators

## Parameter and return value annotation

These annotations are completely optional. It simply makes the program more structured, and maybe provide clues to auto-complete...

e.g.

```python
def foo(x: expression, y: expression = 20) -> float:
    ...
```
Such annotation information will be stored at `func.__anotations__`. E.g. for the above:

```python
>>> foo.__annotations__
{'x': 'expression', 'y': 'expression', 'return': 'float'}
>>> 
```


## Function decorator

Function decorator is used when you want to decorate a function!

(Recall that decorator is used when we want to add behavior to an individual object, dynamically, without affecting the behavior of other objects of the same class.)

E.g.

```python
@some_decorator
def hello_world():
    print("hihi")

# is equivalent to:

def hello_world():
    print("hihi")
hello_world = some_decorator(hello_world)
```

A structure of the decorator could be:

```python
def some_decorator(func):
    def inner(*args, **kwargs):
        print("Before execution")

        returned_value = func(*args, **kwargs)

        print("After execution")

        return returned_value
    return inner
```

### Decorator with parameter

Simple, just:

```python
@some_decorator(param)
def hello_world():
    print("hihi")

# is equivalent to:

def hello_world():
    print("hihi")
hello_world = (some_decorator(param))(hello_world)
```
