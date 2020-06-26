# `pdb`

`pdb` defines an interactive source code debugger.

## Functionality

1. Breakpoint mode

2. Post-mortem mode (Only invoked when abnormal exit code)

## Break-point usage

Insert:

```python
import pdb; pdb.set_trace()
```

## Useful Debugger commands

- `help`

- `where`: Print the stack trace

- `up`, `down`: Move up and down the stack trace

- `ll`: List the source code for current function

- `continue`: Continue until next breakpoint
