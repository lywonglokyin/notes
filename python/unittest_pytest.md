# Python Unittest

## `unittest.mock`

### Why use `unittest.mock`?

It allows some part of the system to be replaced.

- Specific calls to external API or websites (e.g. scraping) (?)

- Slow or troublesome functions (eject disk, network access)

### Related objects and functions

- `Mock`

- `MagicMock`

- `patch()`

# Pytest

A framework to make you test more easily. The design is to use `assert` only in test cases.

## Features

1. `assert` only
2. Auto-discovery of test modules and functions
3. Modular fixtures

## Fixture

### Why use fixture?

**Dependency injection** structure

- Delegate the responsibility (and also right of modification) to the "injector", i.e., the fixture function. Test function therefore becomes a pure consumer of fixture object.

### Basics

Test functions can receive fixture objects by naming them as input argument, and define a corresponding **fixture function** by marking with `@pytest.fixture`. E.g.

```python
import pytest

@pytest.fixture
def sample_fixture():
    return [1,2,3,4,5]

def test_array(sample_fixture):
    assert sample_fixture[0] == 1
```

### Sharing fixture function

Fixture functions can be shared by test files when put in `conftest.py` file, which is then automatically discovered by `pytest` (no import is needed).

### Sharing function across a class, module or session