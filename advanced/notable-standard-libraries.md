# Notable Libraries

This document covers some useful standard libraries in Python.

## Table of Contents

- [`functools`](#functools)

- [`itertools`](#itertools)

- [`dataclasses`](#dataclasses)

- [`enum`](#enum)

[Back to Advanced Topics](/advanced/README.md)

## [`functools`](https://docs.python.org/3/library/functools.html)

`functools` provides additional support for higher-order functions. In Python, this means any `Callable`.

### `functools.lru_cache`

`lru_cache` is a decorator which provides least-recently-used caching for functions.

```python
@lru_cache(max_size=256)
def fibonacci(n: int) -> int:
    if n == 0:
        return 0
    if n == 1:
        return 1

    return fibonacci(n-1) + fibonacci(n-2)

@lru_cache  # unbound cache size
def ackermann(m: int, n: int) -> int:
    if m < 0 or n < 0:
        raise ValueError

    if m == 0:
        return n + 1
    elif n == 0:
        return ackermann(m - 1, 1)
    else:
        return ackermann(m - 1, ackermann(m, n - 1))
    else:
        raise ValueError
```

### `functools.partial`

`partial` allows you to bind positional and keyword arguments to a callable, generating a new higher-order callable.

```python
import functools


def greet_person(name: str, followup: str) -> None:
    print(f"Hello, {name}! {followup}")


greet_graham = functools.partial(greet_person, "Graham")
greet_graham("How are you?")


ask_person_how_they_are = functools.partial(greet_person, followup="How are you?")
ask_person_how_they_are("Graham")
```

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## [`itertools`](https://docs.python.org/3/library/itertools.html)

`itertools` provides a toolbox for building iterators taking heavy inspiration from functional languages such as Haskell.

### `itertools.count`

`count` creates an infinite iterator that counts from a starting number with an optional step.

```python
count_from_zero = itertools.count()

count_from_ten = itertools.count(10)

count_from_30_by_3 = itertools.count(30, 3)
```

### `itertools.chain`

`chain` allows you to take multiple iterators and "chain" them into a single iterator.

```python
for n in itertools.chain([1, 2, 3], [4, 5, 6], [7, 8, 9])
    print(n)  # 1 2 3 4 5 6 7 8 9

# equivalent
iterable_of_iterables = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
itertools.chain.from_iterable(iterable_of_iterables)
```

### `itertools.batched`

`batched` takes an iterable and creates a new iterable that produces batches of a fixed length.

```python
for batch in itertools.batched("ABCDEFGH", n=3):
    print(batch)  # ABC DEF GH
```

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## [`dataclasses`](https://docs.python.org/3/library/dataclasses.html)

`dataclasses` allows generation of classes -- including their magic methods (sometimes called 'special methods') -- via a decorator and type annotations.

```python
from dataclasses import dataclass, field


@dataclass
class Person:
    name: str
    age: int

person = Person("Bob", 42)


# keyword only initialization, include order
@dataclass(kw_only=True, order=True)
class Car:
    make: str = field(compare=False)  # disable these fields for comparisons
    model: str = field(compare=False)
    miles_per_gallon: int

cars = [
    Car("Ford", "F150", 15),
    Car("Toyota", "Corolla", 35),
    Car("Suburu", "Outback", 26),
]
assert cars[1] < cars[0]
assert cars[0] > cars[2]
most_fuel_efficient_car = min(cars)


# 'post init' attributes
@dataclass
class Circle:
    radius: float
    area: float = field(init=False)  # not part of initialization

    def __post_init__(self) -> None:
        """Runs after initializer"""
        self.area = 3.14 * self.radius**2

unit_circle = Circle(1)
assert unit_circle.area == 3.14
```

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## [`enum`](https://docs.python.org/3/library/enum.html)

`enum` provides support for enumeration types.

```python
import enum


class Direction(enum.Enum):
    NORTH = 0
    SOUTH = 1
    EAST = 2
    WEST = 3


# automatic values
class Color(enum.Enum):
    RED = enum.auto()
    GREEN = enum.auto()
    BLUE = enum.auto()
    YELLOW = enum.auto()
    PURPLE = enum.auto()


# ensure unique values
@enum.unique
class DayOfWeek(enum.Enum):
    SUNDAY = 0
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 5  # this will cause an error


# interop with 'int' type
class Number(enum.IntEnum):
    ONE = 1
    TWO = 2
    THREE = 3
    FOUR = 4
    FIVE = 5

assert Number.ONE == 1
assert Number.TWO + Number.THREE == Number.FIVE
assert Number.FIVE * 2 == 10


# int flags
class Permissions(enum.IntFlag):
    READ = enum.auto()  # 0b1
    WRITE = enum.auto()  # 0b10
    DELETE = enum.auto()  # 0b100
    ALL = READ | WRITE | DELETE


# interop with `str` type
class Priority(enum.StrEnum):
    VERY_HIGH = "very_high"
    HIGH = "high"
    MEDIUM = "medium"
    LOW = enum.auto()  # uses lowercase attr name i.e 'low'
    VERY_LOW = enum.auto()  # 'very_low'

assert Priority.HIGH == "high"
assert Priority.VERY_LOW.startswith("v")
```

Python enumerations can also be iterated over.

```python
for priority in Priority:
    print(priority)

directions = list(Direction)
```

Enumerations are singletons, so you use `is` for direct comparison. You can get a singleton instance dynamically by value using intantiation, and by 'name' using indexing on the type.

```python
assert Number(5) is Number.FIVE
assert Number["TWO"] is Number.TWO
```

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)
