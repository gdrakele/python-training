# Built-in Functions and Types

This document introduces some of Python's less esoteric built-in functions and types. It is by no means exhaustive and only seeks to cover the commonly used built-ins.

For a full list of built-ins you can visit the [documentation](https://docs.python.org/3/library/functions.html).

## Table of Contents

- [`True`, `False`, and `None`](#true-false-and-none)

- [`all` & `any`](#all--any)

- [`breakpoint`](#breakpoint)

- [`compile`, `eval`, & `exec`](#compile-eval--exec)

- [`range`](#range)

- [`enumerate`](#enumerate)

- [`filter`](#filter)

- [`help`](#help)

- [`id`](#id)

- [`input`](#input)

- [`len`](#len)

- [`map`](#map)

- [`open`]

- [`reversed`](#reversed)

- [`round`](#round)

- [`sorted`](#sorted)

- [`min` & `max`](#min--max)

- [`sum`](#sum)

- [`zip`](#zip)

- [Types](#types)

[Back to Basic Topics](/basic/README.md)

## `True`, `False`, and `None`

As you would expect Python has values for boolean true, and boolean false: `True` and `False` respectively. Note the capitalization. However since _everything_ in Python is an object, they are actually singleton instances of the `bool` type rather than a compiler level symbol.

```python
true_var = True
false_var = False
```

`None` is another singleton instance which replaces the place of `null` in other languages. As a singleton, you compare against it using `is` rather than `==`. `is` will be explained in more detail later in this document.

```python
if value is None:
    # do something
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## Ellipsis - `...`

## `all` & `any`

`all` and `any` check the truthiness of the values in an iterable and then return a boolean result.

`all` returns `True` iff all elements in the iterable are truthy. `any` returns `True` if any element in the iterable is `True`.

```python
if all(iterable):
    # all values in iterable are truthy

if any(iterable):
    # some value in iterable is truthy
```

When using `all`, `any`, or any similar functions you generally use comprehensions for readability and expressiveness.

```python
if all(num % 2 == 1 for num in iterable):
    # all numbers in iterable are odd

if any(num % 2 == 0 for num in iterable):
    # some number in iterable is even
```

Comprehensions will be explained in more detail later in the training, including the special syntax demonstrated above. This is introduced but not explained early to help solidify best practices when the time is right to explain the above syntax.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `breakpoint`

The `breakpoint` function drops into Python's debugger `pdb`. You can place it anywhere in your code to drop into the debugger at that precise location.

```python
breakpoint()
```

It is important to note that many IDEs will provide more advanced debuggers, but this allows debugging even if you have access to zero tooling. The default debugger it launches is built into Python.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `compile`, `eval`, & `exec`

`compile` and `exec` have similar uses in that they both take string representations of Python code and produce a result. `compile` compiles them into a code object which can be executed by using `exec` whereas `eval` strictly works on statements and directly returns the result.

Their mention here is not to demonstrate their use but as a warning of how dangerous they are. Calling these functions against untrusted input allows for arbitrary code execution and is extremely dangerous.

Online sources will imply there are steps you can take to make this functions safe, but in reality they only make them more difficult to exploit.

These functions can enable incredibly powerful (but hard to reason about) features, but extreme care must be taken in their use. It is almost always better to find an alternative solution.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `range`

`range` generates an iterable of numbers in a defined range. The "end" number of the range is a non-inclusive bound.

It has 3 forms:

```python
range(10)  # 0 -> 9
range(11, 42)  # 11 -> 41
range(0, 100, 3)  # 0 -> 99 in steps of 3; 0, 3, 6, 9, etc
```

It also supports negatives for the step value.

```python
range(10, 0, -1)  # 10 -> 1
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `enumerate`

The `enumerate` function is Python's answer to its lack of traditional C-style for loops.

A common anti-pattern in Python when you wish to iterate over an iterable with indexes is the following:

```python
for i in range(len(iterable)):
    print(f"index {i} - {iterable[0]}")
```

`enumerate` solves this issue by wrapping an iterable and producing (index, value) tuples for each element. When combined with iterable unpacking, the correct pattern becomes:

```python
for idx, value in enumerate(iterable):
    print(f"index {idx} - {value}")
```

`enumerate` also allows you to override the starting index.

```python
for idx, value in enumerate(iterable, start=42):
    ...
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `filter`

`filter` wraps an iterable and uses a predicate function to filter the elements.

```python
def is_even(num):
    return num % 2 == 0

for num in filter(is_even, range(10)):
    print(f"{num} is even")
```

If you use the `None` singleton instead of a predicate it filters based on the 'truthiness' of the elements.

```python
for truthy_value in filter(None, iterable):
    ...
```

The general consensus of the community is that conditional comprehensions should be used over `filter` for most use cases.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `help`

`help` is used to print helpful information about any Python object.

By using docstrings, a feature of Python that will be discussed later, you can control the help messages for your own objects.

```python
help(object)
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `id`

The `id` function returns a guaranteed unique integer identifier for a Python object.

```python
id(object)
```

If the IDs of two objects are the same, they are the same object in memory by definition.

```python
id(a) == id(b)
```

The `is` statement is shorthand for this equality comparison

```python
a is b
```

The return value of `id` is actually the integer representation of the objects address in memory.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `input`

The `input` function blocks on stdin and waits for user input. It supports an optional prompt.

```python
promptless_user_input = input()
prompted_user_input = input("Name? ")
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `len`

`len` simply returns the length of an iterable.

```python
len(iterable)
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `map`

`map` transforms an iterable by applying a function to each element.

```python
def double(num):
    return n * 2

map(double, range(5))
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `open`

The `open` function opens file descriptors for reading and writing. It takes an optional `mode` parameter which takes a string representing the mode for the file to be opened with a default of 'rt' or for read text mode.

```python
# equivalent
open("some/file/to.open")
open("some/file/to.open", mode="r")
```

Mode strings are created by composing certain directive characters listed below. The characters are delimited by where in the mode string they can appear.

```
r: read mode  (default)
w: write mode
x: write mode (error if file already exists)
a: append mode
---
+: read/write
---
b: binary mode
t: text mode  (default)
```

The following are some example modes:

```
open(..., mode="r")  # read a file as text
open(..., mode="r+")  # read/write file as text
open(..., mode="a+b)  # read/append file as binary
open(..., mode="xb")  # create the file for writing as binary
```

Python's file descriptors have many methods for interaction, some of the most common are as follows:

```python
infile = open("path/to/some.file", mode="r")
infile.read()  # read all the content of the file
infile.read(1024)  # read up to 1024 characters
infile.readlines()  # list of lines in the file
infile.close()

outfile = open("path/to/some.file", mode="w")
outfile.write("data to write into the file")
outfile.writelines(["line 1\n", "line 2\n", "line 3\n"])
outfile.close()
```

The python way to open an interact with a file is using a context manager which will handle closing the file for you, even if errors occur during processing.

```python
with open("path/to/some.file") as infile:
    data = infile.read()
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `reversed`

`reversed` simply wraps an iterable in a new iterable that iterates over the elements in reverse order.

```python
for value in reversed([1, 2, 3, 4]):
    print(value)
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `round`

The `round` function rounds a number, truncating it's fractional part.

```python
round(2.5)  # 2
```

It supports an optional 'ndigits' parameter for controller the digits after the decimal.

```python
round(2.567, 2)  # 2.57
```

`round` uses banker's rounding, which rounds numbers equidistant from the target precision to the nearest even integer.

```python
round(1.5)  # 2
round(2.5)  # also 2!
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `sorted`

The `sorted` function takes an iterable and returns a list of the elements sorted.

```python
sorted_names = sorted(names)
```

The `sort` function also optionally takes a `key` parameter which expects a function that takes an element in the iterable and returns the value that should be used for comparison.

```python
people = [
    ("Bob", 50),
    ("Sue", 30),
    ("John", 40),
]

def people_age_key(person):
    return person[1]

# sorted by age
sorted(people, key=people_age_key)
```

You can also pass a 'reverse' parameter to reverse the sort order to descending.

```python
sorted(iterable, reverse=True)
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `min` & `max`

`min` and `max` return the minimum or maximum element in either an iterable _or_ of a sequence of items.

```python
min([1, 2, 3])
min(1, 2, 3)
max([4, 5, 6])
max(4, 5, 6)
```

Both raise an error if given an empty iterable, however an optional 'default' parameter can be used to specify a return value in such cases.

```python
min([], default=0)
max([], default=42)
```

Like `sort, `min` and `max` accept an optional 'key' parameter that can be used to override how the values for comparison are calculated.

```python
people = [
    ("Bob", 50),
    ("Sue", 30),
    ("John", 40),
]

def people_age_key(person):
    return person[1]

min(people, key=people_age_key)
max(people, key=people_age_key)
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `sum`

`sum` computes the sum of all elements in an iterable.

```python
sum(iterable)
```

You can optionally pass a 'start' parameter -- which has a default of '0' -- to `sum`.

```python
sum(iterable, start=42)
```

`sum` is specifically designed to work with numerical types and may reject non-numeric types according to its documentation. In practice however it often functions correctly as long as you override the 'start' parameter to the alternative type.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## `zip`

The `zip` function takes 1 or more iterables and pairs their elements into N-length tuples, where N is the number of iterables.

```python
for a, b, c in zip(iterable1, iterable2, iterable3):
    ...
```

The maximum number of zipped tuples is determined by the length of the shortest iterable.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## Types

The following section covers the basic built-in data types, separated into atomic types and collection types.

This section is not exhaustive, but seeks to cover the primary types you would use for writing a solution without delving into the types that require more obscure use.

### Atomic Types

This section covers Python's 5 atomic types: `bool`, `bytes`, `float`, `int`, `str`.

#### `bool`

Python's boolean type has 2 singleton instances as stated previously, `True` and `False`.

The `bool` callable can be used to convert any object into one of these singletons corresponding to the "truthiness" of that object.

```python
# falsy
bool(0)
bool("")
bool([])  # empty collections are 'falsy'

# truthy
bool(1)
bool(100)
bool("hello")
bool([1, 2, 3])
bool(object)
bool(object())
```

Python's if-statements, and the `and`, `or`, and `not` operators implicitly use `bool` to determine the truthiness of expressions, so there is no need to coerce values to booleans _or_ explicitly compare to the boolean singletons.

```python
if []:  # false, empty collection
    ...
if 0:  # false
    ...
if "hello":  # true
    ...
```

#### `bytes`

`bytes` are immutable sequences of integers that are represented as characters, much like strings.

They can be specified using bytes literals.

```python
b"this is a bytes literal"
```

The `bytes` callable can coerce supported types into bytes objects.

```python
bytes("hello", encoding="utf-8")  # strings require an encoding
bytes([104, 101, 108, 108, 111])
bytes(10)
```

#### `float`

The `float` type is extremely similar to C's, with the same syntax used for literals.

```python
1.5
```

You can use the `float` callable to convert other supported objects into floats.

```python
float(True)
float(1)
float("13.37")
```

#### `int`

`int` literals borrow their literal syntax for C.

```python
42
```

The `int` callable can convert supported objects into integers.

```python
int(1.5)
int(False)
int("1337")
```

Python instantiates the integers -5 to 256 on startup and reuses them for all instances of these values for speed and efficiency.

#### `str`

`str` in Python are immutable sequences of characters who again borrow their syntax for literals from C, with an important divergence: double and single quotes in Python are interchangeable.

```python
"Hello, World!"
'Hello, World!"
```

The `str` callable can be used to convert other objects into their string representations. All types are supported in this conversion.

```python
str(1)
str(1.5)
str(True)
str(None)
str([1, 2, 3])
```

Python strings are interned to save memory and improve efficiency.

#### What about `char`?

Python has no explicit `char` type. Instead you use strings/bytes of length 1 or an integer representing the byte anywhere you would expect to use `char` depending on the context.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

### Collection Types

This section covers Python's 5 built-in collection types: `list`, `dict`, `tuple`, `set`, and `bytearray`.

#### `list`

Lists are mutable sequences, analogous to arrays in C.

```python
example_list1 = [1, 2, 3, 4]
example_list2 = []
example_list2.append(5)
example_list2.append(6)
example_list2.append(7)
example_list2.append(8)
example_list1[0] = 0
```

Python also provides special syntax for indexing from the end of a list using negative indexes.

```python
example_list2[-1] = 9  # sets the last element's value
```

Lists double as a stack data structure using the `list.append` and `list.pop` methods.

```python
stack = [1, 2, 3, 4]
stack.append(4)
stack.append(5)
stack.pop()
```

Lists support slicing, a feature supported by all of Python's standard library sequences. Slices are 1 to 3 numbers seperated by colons -- ':' -- to specify 'start:end:step'.

```python
example_list[1:]  # index 1 to the end of the list
example_list[1:4]  # index 1 to 3, 'end' is not inclusive
example_list[1:100:3]  # indexes 1 to 99 in steps of 3
example_list[1:-2]  # index 1 to the second to last element
example_list[10:1:-1]  # index 10 to 2 in reverse order
```

The `list` callable can be used to consume any iterable and generate a list from the elements.

```python
list(iterable)
```

#### `dict`

Dictionaries are key-value hash maps.

```python
example_dict = {
    "key": "value",
    "another_key": "another_value",
    "number": 1,
    "list": [1, 2, 3, 4],
    "nested": {
        "yet_another_key": "yet_another_value"
    },
    "key": "dup"
}
```

Dictionary keys must be hashable. Atomic types are hashable, but mutable types are not. 

```python
# raises an exception because lists are not hashable values.
{[]: "value"}
```

Values can be accessed using `[]` indexing. You can also use the `dict.get` method if you want fallback values if a key is missing.

```python
example_dict["key"]
example_dict["number"]
example_dict.get("list")
example_dict.get("not_found")  # default fallback value is None
example_dict.get("not_found", "default")
```

Dictionaries provide several iterator methods.

```python
for key in dictionary.keys():
    ...

for value in dictionary.values():
    ...

for key, value in dictionary.items():
    ...
```

The `dict` callable can be used to instantiate dictionaries _and_ create dictionaries from iterables. Iterable conversion requires elements to themselves be 2-length iterables representing key-value pairs, usually 2-tuples.

```python
dict(key="value", num=1)
dict([("key", "value"), ("num", 1), (True, False), (1.0, None)])
```

#### `tuple`

Tuples are immutable sequences of objects. `tuple` literals use parentheses, though in unambiguous contexts they are optional.

```python
example_tuple1 = (1, 2, 3, 4)
example_tuple2 = 1, 2, 3, 4
```

Due to their immutability, tuples are hashable and can be used to create composite dictionary keys.

```python
composite_key_dict = {(1, "two", 3.0): "value"}
```

The `tuple` callable can be used to consume any iterable and generate a list from the elements.

```python
tuple(iterable)
```

Like lists, tuples also support slicing.

#### `set`

Python's sets are analogous to sets from mathematics. They use curly brackets for literals.

```python
example_set = {1, 2, 3, 4}
example_set.add(5)
example_set.add(1)
example_set.remove(2)
```

Implemented as hash sets, elements must be hashable.

```python
# raises an exception because lists are not hashable values.
{[]}
```

Sets support mathematical operations for set theory.

```python
s1 & s2  # union
s1 & s2  # intersection
s1 - s2  # difference
s1 ^ s2  # symmetric difference
```

#### `bytearray`

Byte arrays are mutable sequences of bytes and Python's answer to C-style strings.

```python
bytearray(b"Hello, World!")
```

Once instantiated they only support integer values representing bytes when mutating.

```python
example_barray = bytearray(b"graham")
example_barray[0] = 71
```

You can use the `ord` function to convert characters to their integer equivalents for such purposes.

```python
example_barray = bytearray(b"graham")
example_barray[0] = ord("G")
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)