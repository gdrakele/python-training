# Python's Type System

This document seeks to explain Python's typing system and its ramifications.

Python is a strong, dynamically typed language that makes use of duck typing.

## Table of Contents

- [Strong Typing](#strong-typing)

- [Dynamic Typing](#dynamic-typing)

- [Duck Typing](#duck-typing)

- [Ramifications](#ramifications)

[Back to Basic Topics](/basic/README.md)

## Strong Typing

As a strongly typed language, the types of variables matter when attempting operations.

Type coercion is not performed to try and make sense of sense of an expression or statement, rather an error is raised.

```python
1 + 1
1 + 1.0
1 + "1"  # error
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## Dynamic Typing

Dynamic typing allows you to change the type of a variable.

```python
value = 1
value = "1"
value = 1.0
value = None
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## Duck Typing

_**"If it looks like a duck, and quacks like a duck, then it's a duck!"**_

Duck typing means that Python cares about what an object _can do_ rather than what _it is_.

This allows you to pass whatever object you want to a function, method, or other callable and as long as that object _can do_ what is expected the operation will succeed.

This does mean that you can pass an incompatible object and Python _will try_ to make it work, and an error will be raised at runtime.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## Ramifications

The typing system of Python is in my opinion Python's greatest strength _and_ it's greatest weakness.

It makes the exploratory phase of programming significantly faster, sometimes orders of magnitude faster. This greatly accelerates the pace of learning and resolving implementation issues before finally writing production code.

As a side effect this allows you to write code that is very hard to follow, or overly complex. This pitfall can be avoided by using 'Type Annotations', a topic we will cover in intermediate training.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)
