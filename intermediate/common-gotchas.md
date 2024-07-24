# Common Gotchas

This document covers a couple of Python's most common gotchas.

## Table of Contents

- [Recursion Limits](#recursion-limits)

- [Mutable Default Parameters](#mutable-defaultsparameters)

[Back to Intermediate Topics](/intermediate/README.md)

## Recursion Limits

Python has a hard coded recursion limit to prevent infinite recursion from overflowing the C stack and crashing the interpreter.

```python
import sys

print(sys.getrecursionlimit())
```

If the number of current stacks exceeds this number a `RecursionError` is raised. In Python 3.12 this defaults to 3,000.

This value can be adjusted by the very brave.

```python
import sys


sys.setrecursionlimit(10_000)
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Mutable Defaults Parameters

When using default parameter values, care must be taken to not use mutable values.

```python
def foo(default_param: list[int] = []) -> None:
    ...
```

All calls of the function share the same reference, if the default is mutated it will effect all future calls.

As an alternative, use `None` and manually set the default mutable value you require.

```python
def foo(default_param: list[int] = None) -> None:
    if default_param is None:
        default_param = []
    
    ...
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)
