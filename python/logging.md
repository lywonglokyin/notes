# Logging

Logging can generally be done with the python standard library `logging`.

## Log level

There are in total 5 level for logs in Python. They are

- `DEBUG`
- `INFO`
- `WARNING`
- `ERROR`
- `CRITICAL`

The default display level of `logging` is `WARNING`.

## Loging to a file

Can edit the `basicConfig` of the `logging`:

```python
logging.basicConfig(filename="sample.log")
```

