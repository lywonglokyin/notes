# Defered

`Defered` is a mechanism provided in python in `twisted.internet.defer.Deferred`.

## Asynchronous operations

Normally in python, statement is executed in order. However in case of `exceptions`, the order of execution will become unclear, and would have to be enforced by `try`, `except` and `finally`. Asynchronous operation is another way of thinking that allows statements to run concurrently, but only invoke certain operation after some operations 

## Defered - twisted 

Old code:
```python
pod_bay_doors.open()
pod.launch()
```

New code:
```python
d = pod_bay_doors.open()  # d is a deferred object
d.addCallback(lambda ignored: pod.launch())
```

### Or failure handling...

Old code:
```python
try:
  y=f()
except Exception as e:
  y=g(e)
h(y)
```

New code:
```python
d = f()
d.addErrback(g)
d.addCallback(h)
```

OR

```python
d = f().addErrback(g).addCallback(h)
```

And also...

Old code:
```python
try:
  y = f()
finally:
  g()
```

New code:
```python
d = f()
d.addBoth(g)
```

... https://twistedmatrix.com/documents/current/core/howto/defer-intro.html
