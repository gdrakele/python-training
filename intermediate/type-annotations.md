# Type Annotations

This document introduces type annotations (also called "type hinting").

Full documentation of Python's type annotations can be found by reading [PEP 484 - Type Annotations](https://peps.python.org/pep-0484/), [PEP 526 - Syntax for Variable Annotations](https://peps.python.org/pep-0526/), and the documentation for the [`typing`](https://docs.python.org/3/library/typing.html) library.

## Table of Contents

- [Introduction](#introduction)

- [Using String Literals as Stand-Ins and The`__future__` Module](#using-string-literals-as-stand-ins-and-the__future__-module)

- [Union Types](#union-types)

- [Type Aliases](#type-aliases)

- [Generics & `TypeVar`](#generics--typevar)

[Back to Intermediate Topics](/intermediate/README.md)

## Introduction

Type annotations in Python allow you to annotate variables, parameters, and classes with types for use by your IDE, tooling, and other developers.

```python
def fibonacci(num: int) -> int:
    if num == 0:
        return 0
    elif num == 1:
        return 1
    
    return fibonacci(n - 1) + fibonacci(n - 2)
```

Type annotations for variables follow the same syntax.

```python
name: str
age: int
salary: float
```

It is considered bad form to annotate a variable with an initializing value that correctly identifies the desired type. Rather allow the type to be inferred.

```python
# do this
name = "Bob
age = 42
salary = 1337.5

# don't do this
name: str = "Sarah"
age: int = 32
salary: float = 5733.1
```

Generic types, such as containers, can have their type set using square brackets after the type.

```python
list_of_names: list[str]
```

Many utilities for typing annotations can be found in Python's standard library's `typing` module.

```python
import typing
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Using String Literals as Stand-Ins and The`__future__` Module

Sometimes when providing types annotations a symbol is not yet defined. This can happen with recursive types or when referencing a class within its own definition.

To accommodate this situation Python and the available type annotation tooling supports string literals as stand-ins for types.

```python
value: "RecursiveType"
```

To alleviate this a future feature of Python will be to defer type annotation processing.

```python
value: RecursiveType
```

A module exists called `__future__` which supports importing future not-yet-default features or backporting functionality. It can be used to enabled deferred annotations in the importing module.

```python
from __future__ import annotations
```

Only the import statement is necessary to enable the feature.

This doesn't fix every required usage of stand-in string literals, but will resolve most cases.

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Union Types

Union types in Python receive more use in the type system than other languages for 2 primary reasons:

1. Dynamic typing makes it easy for types to change

2. Python's references are not nullable, rather you simply set the value to the `None` singleton

As of Python 3.10, union types can be specified using the pipe '|' character.

```python
salary: float | None = None
hourly_wage: float | None = 50.0
```

In older versions of Python, or when you can't use '|' such as when you must use a string literal, you can instead use the `Union` member of the `typing` module.

```python
from typing import Union

salary: Union[float, None] = None
hourly_wage: Union[float, None] = 50.0
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Type Aliases

Type aliases are useful for improving readability, expressiveness and/or condensing large composed types into a smaller form.

They are as simple as assigning a type a new name as you would with any variable.

```python
# readability / expressiveness
DeviceId = int
JsonAtom = bool | float | int | str | None

# condense a larger type
# mean/median/mode temperature of weather stations
WeatherStationTemps = dict[str, list[tuple[float, float, float]]]
```

The `typing` module also includes a `TypeAlias` member that can be used to annotate a type alias when you are forced to use a string literal stand-in.

```python
from typing import TypeAlias

MyAlias: TypeAlias = "SomeTypeRequiringStandIn"
``` 

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Generics & `TypeVar`

Python supports generics by way of `typing.TypeVar`. A `TypeVar` is an unbound type placeholder, used in a type annotation, to signify that that parameter / variable / attribute is generic.

```python
from typing import TypeVar

T = TypeVar("T")  # The variable name and given string must be the same

def last_element(sequence: list[T]) -> T:
    return sequence[-1]
```

You can also bind a specific base class, or control the expectation for covariance or contravariance using the `bound`, `covariant`, and `contravariant` keyword arguments respectively.

```python
from typing import TypeVar


SpecialT = TypeVar("SpecialT", bound=Special)
T_co = TypeVar("T_co", covariant=True)
T_contra = TypeVar("T_contra", contravariant=True)
```

As of Python 3.12 new syntax for specifying generics exists, which removes the need for defining explicit `TypeVar`s.

```python
def last_element[T](sequence: list[T]) -> T:
    return sequence[-1]
```

Most of Python's type annotation tooling is still working on supporting this new syntax, so at this time its use it not recommended. 

While `TypeVar`'s can be reused to create other generics, doing so prevents you from using descriptive names and can hurt the readability of the code. Do so with care.

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)
