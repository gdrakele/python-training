# Advanced Data Model

This document introduces some advanced concept in Python's data model.

## Table of Contents

- ["Right" Binary Arithmetic Operators](#right-binary-arithmetic-operators)

- [`__new__`](#__new__)

- [Metaclasses](#metaclasses)

[Back to Advanced Topics](/advanced/README.md)

## Reflected Operators

All the binary arithmetic operators support a swapped variant so that handling can be done by the _right operand_ if the _left operand_ does not provide support.

These swapped operators all have an "r"-prefix in front of the dunder methods name: `__radd__`, `__rsub__`, `__rlshift__`, etc.

To implement an operator for some types, but still play nicely with the swapped operators you can return `NotImplemented` when you do not support the operation.

Since Python's built-in types all properly support these semantics you can extend operators with them using your own types with the swapped operators.

```python
from typing import Any, Self


class AddsWithIntegers:
    def __init__(self, value: int) -> None:
        self.value = value

    def __add__(self, other: Any) -> Self:
        if isinstance(other, int):
            return AddsWithIntegers(self.value + other)

        return NotImplemented

    def __radd__(self, other: Any) -> Self:
        return self + other  # fall back on '__add__'

    def __sub__(self, other: Any) -> Self:
        if isinstance(other, int):
            return AddsWithIntegers(self.value - other)

        return NotImplemented

    def __rsub__(self, other: Any) -> Any:
        if isinstance(other, int):
            return AddsWithIntegers(other - self.value)

        return NotImplemented
```

There are no "r"-prefixed reflected operators for the comparison operators as their reflections already exist.

- `__lt__` and `__gt__` are each other's reflections

- `__le__` and `__ge__` are each other's reflections

- `__eq__` and `__ne__` are their own reflections

Some prioritization done to determine the order in which methods on the left or right operand are tried for comparison operators. Specifically the right operand's reflected method has priority when it is a direct or indirect subclass of the left operand. In other cases the left operand's method has priority.

We can extend our `AddsWithIntegers` example above to compare with `int`s as follows:

```python
class AddsWithIntegers:
    ...  # previous code

    def __eq__(self, other: Any) -> bool:
        if isinstance(other, int):
            return self.value == other

        return NotImplemented
```

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## `__new__`

While `__init__` was previously explained to only be an _initializer_, Python does have a constructor called `__new__`. It functions as a factory and Python provides a built-in one, courtesy of the root object `object` which is sufficient for almost all use cases.

`__new__` is implicitly a `classmethod` and starts with `cls` rather than `self` but does not require decoration with `@classmethod`.

You can extend `__new__` for a myriad of cases but one of the most common is to implement a singleton.

```python
from typing import ClassVar, Self


class Singleton:
    _instance: ClassVar[Self | None] = None

    def __new__(cls) -> Self:
        if cls._instance is None:
            cls._instance = super().__new__(cls)

        return cls._instance
```

Extended implementations of `__new__` are passed the same parameters as `__init__`. These are _not_ passed to `object.__new__`.

```python
from typing import Any, ClassVar, Self


class Singleton:
    _instance: ClassVar[Self | None] = None

    def __init__(self, value: Any) -> None:
        self.value = value

    def __new__(cls, value: Any) -> Self:
        if cls._instance is None:
            cls._instance = super().__new__(cls)

        return cls._instance
```

A common practice, especially when you do not need to inspect these values, is to use `*args` and `**kwargs` as catch-alls so you don't need to adjust the signature of `__new__` as `__init__` changes.

```python
from typing import Any, ClassVar, Self


class Singleton:
    _instance: ClassVar[Self | None] = None

    def __init__(self, value: Any) -> None:
        self.value = value

    def __new__(cls, *args: Any, **kwargs: Any) -> Self:
        if cls._instance is None:
            cls._instance = super().__new__(cls)

        return cls._instance
```

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## Metaclasses

Everything in Python is an `object`, including classes. Since all objects must be an instance of something, all classes are instances of their metaclass.

The root metaclass is `type`.

To create your own metaclasses, you only need to inherit from `type` and then specify your metaclass when making new classes.

```python
class MyMetaclass(type): pass

class Example(metaclass=MyMetaclass): pass
```

All object-oriented mechanisms in Python function using metaclasses and the classes that use them.

```python
class MyMetaclass(type):
    def essentially_a_classmethod(self): pass
```

You can construct instances of metaclasses (and therefore make new classes) using the normal object instantiation syntax of Python.

```python
class Example:
    def __init__(self, x: int) -> None:
        self.x = x

    def foo(self) -> None:
        print(f"foo with {self.x=}")

# equivalent to

def example_init(self, x: int) -> None:
    self.x = x

def example_foo(self) -> None:
    print(f"foo with {self.x=}")

name = "Example"  # display name
bases = tuple()  # no inheritance
namespace = {  # contents of the class body
    "__init__": example_init,
    "foo": example_foo
}
Example = type(name, bases, namespace)
```

Metaclasses are often used to provide automatic class setup and other "magic" and is heavily used by some of Python's most popular libraries. 

Metaclasses tend to add significant complexity and thus their use should be restricted to such cases where it is required.

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)
