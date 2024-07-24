# Intermediate Python Syntax & Semantics

This document covers more intermediate Python syntax and semantics.

## Table of Contents

- ["Main" Functions](#main-functions)

- [Decorators](#decorators)

- [Classes](#classes)

- [Magic Methods](#magic-methods)

- [Generators](#generators)

- [Packing and Unpacking With `*` And `**`](#packing-and-unpacking-with--and-)

- [Assignment Expressions](#assignment-expressions)

[Back to Intermediate Topics](/intermediate/README.md)

## "Main" Functions

Python files are executed top-to-bottom, even when imported. This becomes a problem when you want to test a script (or import from it!) and the whole file executes.

To alleviate this and create a main entry point for a script, while keeping it testable and importable, we use the `__name__` attribute that all files have. This attribute is equal to the module path of the file, but when a file is the _root file_ being executed it is equal to "\_\_main__".

```python
if __name__ == "__main__":
    ...  # code
```

A best practice is to make a "main" function that takes no parameters that is invoked inside the above if statement.

```python
def main() -> None:
    ...  # code

if __name__ == "__main__":
    main()
```

### `__main__.py` As a Package Entry Point

Python's packages can also be executed. When executed instead of running the `__init__.py` file, it looks for a special `__main__.py` file in the package and executes that.

To execute a package, use the `-c` flag with Python and the _package path_ using period separators instead of the file path.

```bash
python -c my.cool.package
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Decorators

Python has built in syntactic sugar for applying decorators using '@'.

```python
# equivalent to 'foo = decorator(foo)'
@decorator
def foo() -> None:
    pass

# equivalent to
#   decorator = decorate()
#   bar = decorator(bar)
@decorate()
def bar() -> None:
    pass
```

Decorators are used often in Python for anything from defining static methods to registering HTTP handler functions in web frameworks.

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Classes

Classes in Python are specified using the `class` keyword.

```python
class MyClass:
    pass
```

You can specify an initializer using the special `__init__` method.

```python
class MyClass:
    def __init__(self, value: int) -> None:
        self.value = value
```

The initializer is _not_ a constructor. Python automatically creates an instance factory that is automatically called during instantiation and the constructed instance is then passed to the initializer.

Instance creation is just calling the new class with the initializing parameters.

```python
instance = MyClass(10)
```

### Class Bodies As Executed Namespaces

Class bodies are executed fully as any script, though any definitions are scoped to the classes namespace.

```python
class MyClass:
    name = "Graham"
    print("Hello, World!")  # prints when the class body executes

print(MyClass.name)
```

Any valid Python can be used within a class body, including other classes. Care should be taken to understand that variable will remain bound as attributes on the class.

### Methods, Explicit “Self”, and Bound Methods

The first parameter to a method always represents the class instance. While it can have any name, the convention is to use "self".

```python
class MyClass:
    def __init__(self, value: int) -> None:
        self.value = value
```

When an instance is created the methods "self" parameter is bound to the instance and it becomes a "bound method" that carries the instance with it. Before this, methods are just regular functions within the class's namespace.

A side effect of this is that an instance's methods can be passed by reference -- such as for a callback -- and called elsewhere where the invocation will apply to the bound instance. 

```python
from time import sleep
from collections.abc import Callable


class WorkItem:
    def __init__(self):
        self.done = False
    def complete(self):
        self.done = True

def wait_then_run_callback(self, callback: Callable[[], None]) -> None:
    sleep(10)
    callback()

work_item = WorkItem()

wait_then_run_callback(work_item.complete)
print(work_item.done)
```

This also means that unbound methods referenced using _the class and not an instance_ can be applied to instances. This is useful for functional programming.

```python
names = [
    "bob",
    "sarah",
    "john",
    "sue"    
]

for capitalized_name in map(str.capitalize, name):
    print(capitalized_name)
```

### `staticmethod`, `classmethod`, and `property`

Static methods can be made on classes using the `staticmethod` decorator.

```python
class MyClass:
    @staticmethod
    def foo() -> None:
        print("no instance required!")
```

Class methods can be defined in a similar way using the `classmethod` decorator. Class methods replace the 'self' parameter with a 'cls' parameter that is given the class instance itself. This is most readily used for custom instantiators that automatically respect inheritance.

```python
from typing import Self

class MyClass:
    def __init__(self, value: int) -> None:
        self.value = value

    @classmethod
    def from_file(cls, filepath: str) -> Self:
        with open(filepath) as infile:
            data = infile.read()

        return cls(int(data))
```

Properties can be defined using the `property` decorator.

```python
from math import pi


class Circle:
    def __init__(self, radius: float) -> None:
        self.radius = radius

    @property
    def area(self) -> float:
        return pi * self.radius ** 2


circle = Circle(4)
print(circle.radius)
```

Once decorated, the property method will expose a `setter` decorator attribute that can be used to define the setter.

```python
from math import pi


class Circle:
    def __init__(self, radius: float) -> None:
        self.radius = radius

    @property
    def area(self) -> float:
        return pi * self.radius ** 2

    @area.setter
    def area(self, area: float) -> None:
        self.radius = (area / pi) ** (1/2)
```

### Inheritance

Python supports multiple inheritance. To invoke a parent's method you use the `super` function.

```python
class Child(Parent, OtherParent):
    def __init__(self) -> None:
        super().__init__()
```

Alternatively you can reference the superclass's unbound methods directly, which can help in cases of multiple inheritance.

```python
class Child(Parent, OtherParent):
    def __init__(self) -> None:
        Parent.__init__(self)
        OtherParent.__init__(self)
```

When determining what parent to use when using `super`, Python uses a concept called "method resolution order" or "MRO". The MRO is closer in concept to "next ancestor" than "parent" and provides a solution to the diamond inheritance problem.

```python
class A:
    def __init__(self) -> None:
        print("A")

class B(A):
    def __init__(self) -> None:
        super().__init__()
        print("B")

class C(A):
    def __init__(self) -> None:
        super().__init__()
        print("C")

class D(C, B):
    def __init__(self) -> None:
        super().__init__()
        print("D")
```

### Generic Classes

To make a class fully generic so it supports the `[<type>]` syntax you use `typing.Generic` in addition to `typing.TypeVar`.

```python
from typing import Generic, TypeVar

T = TypeVar("T")

class MyGenericClass(Generic[T]):
    ...

concrete: MyGenericClass[int] = ...
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Magic Methods

Python's mechanism for hooking into the language features is called "magic methods". Magic methods are a form of "dunder" (double underscore) methods, which are methods whose name have "__" as both a prefix and suffix. This allows you to implement the results of not only operators but also built-in functions for your classes.

Though you are not forced to program in an object oriented style, everything in Python is an object. To hook into the language features you only need implement the required magic methods for your classes.

```python
from types import TracebackType
from typing import Any, Self

class MyClass:
    # --  comparison  --
    # '=='
    def __eq__(self, other: Self) -> bool:
        ...

    # '!='
    def __ne__(self, other: Self) -> bool:
        ...

    # '<'
    def __lt__(self, other: Self) -> bool:
        ...

    # '<='
    def __le__(self, other: Self) -> bool:
        ...

    # '>'
    def __gt__(self, other: Self) -> bool:
        ...

    # '>='
    def __ge__(self, other: Self) -> bool:
        ...

    # --  numeric operators  --
    # '+'
    def __add__(self, other: Self) -> Self:
        ...

    # '-'
    def __sub__(self, other: Self) -> Self:
        ...

    # '*'
    def __mul__(self, other: Self) -> Self:
        ...

    # '/'
    def __truediv__(self, other: Self) -> Self:
        ...

    # '//'
    def __floordiv__(self, other: Self) -> Self:
        ...

    # --  built-ins  --
    # 'len(obj)'
    def __len__(self) -> int:
        ...

    # 'str(obj)', also print(obj)
    def __str__(self) -> str:
        ...

    # 'with obj:'
    def __enter__(self) -> Any:
        ...

    def __exit__(self, exc_type: type[Exception], exc_value: Exception, traceback: TracebackType) -> None:
        ...
```

These can be leverage to provide arbitrary functonality if you wish. A good example is the `pathlib` module which provides object oriented file system paths. The "/" operator is used to implement path concatenation.

```python
from pathlib import Path

cwd = Path.cwd()  # current working directory
filepath = cwd / "path" / "to" / "some.file"
```

This list is by no means exhaustive. Full documentation of the available magic methods can be found by reading about Python's [data model](https://docs.python.org/3/reference/datamodel.html).

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Generators

Generators are functions that behave like iterators. They use 'yield' instead of 'return'. Any function with a 'yield' will return a generator when called.

```python
from collections.abc import Iterator
def lazily_double_list_elements(numbers: list[int]) -> Iterator[int]:
    for num in numbers:
        yield num * 2

numbers = [1, 2, 3, 4, 5]
for doubled_num in lazily_double_list_elements(numbers):
    print(doubled_num)
```

You can store the returned generator in a variable to pass around as you see fit.

```python
gen = lazily_double_list_elements(numbers)
for doubled_num in gen:
    print(doubled_num)
```

If you wish to manually iterate over a generator you can use the build-in function `next`. Just capture the `StopIteration` exception to know when iteration is complete.

```python
try:
    next(gen)
except StopIteration:
    print("no values left in generator!")
```

Generators can only be consumed a single time.

### Generator Comprehensions

Using parentheses for a comprehension results in a generator comprehension that will only compute the elements as you request them.

```python
doubled_num_gen = (x*2 for x in range(10))
```

This is very useful for creating pipelines of comprehensions that don't require extra memory if you know the intermediate (or even final!) results need only be used for a single iteration.

If you put a single generator comprehension in a function call with no other parameters you can use the functions parentheses and the generator will be given as the first argument.

```python
if any(not age.isdigit() for age in ages):
    print("one of the ages is not numerical!")
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Packing and Unpacking With `*` And `**`

Python allows you to unpack iterables into the arguments of any callable using `*`.

```python
args = [1, 2, 3]
foo(*args)  # same as foo(1, 2, 3)
```

You can unpack a iterable mapping into keyword arguments in the same way using `**`.

```python
kwargs = {"a": 1, "b": 2, "c": 3}
bar(**kwargs)  # same as bar(a=1, b=2, c=3)
```

These can be combined into one call.

```python
baz(*args, **kwargs)
```

You can also reverse them in function definitions to pack positional arguments and keyword arguments into single variables.

```python
def example(*args: int, **kwargs: str) -> None:
    ...
```

You can have any number of positional parameters before the use of a `*` parameter. The same is true of keyword parameters and a `**` parameter.

```python
def another_example(pos1: float, pos2: bool, *args: int, kw1=10, kw2="hello", **kwargs: str) -> None:
    ...
```

You can also use `*` when unpacking an iterable into multiple variables to capture any remaining elements. It can only appear once.

```python
first, *rest = iterable

first, *middle, last = iterable

*front, last = iterable
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Assignment Expressions

Python added the ':=` operator for assignment expressions, called the walrus operator by the community.

```python
import re

if (match := re.fullmatch(r"\d+", some_string)) is not None:
    ...  # process the regex match
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)
