# Intermediate Labs

This document contains problems relevant to the knowledge learned during the intermediate training.

## Table of Contents

- [JSON Type Alias](#json-type-alias)

- [Vector Type](#vector-type)

[Back to Intermediate Topics](/intermediate/README.md)

## JSON Type Alias

Create a type alias for JSONable types. This will require some intermediary type aliases as well as stand-in string literals due to type recursion.

Start from here:

```python
JsonAtom = bool | float | int | str | None

Jsonable = ...
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Vector Type

Create a 3-dimensional vector type that supports the `+`, `-`, and `*` operators. Additionally implement `str(vec)`.

- `+` should add 2 vectors

- `-` should subtract 2 vectors

- `*` should compute the dot product of 2 vectors

- `str(vec)` should print "Vector3(x=..., y=..., z=...)"

Start from here:

```python
class Vector3:
    def __init__(self, x: float, y: float, z: float) -> None:
        self.x = x
        self.y = y
        self.z = z
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)
