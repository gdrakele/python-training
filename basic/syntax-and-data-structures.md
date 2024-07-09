# Basic Python Syntax & Data Structures

This document introduces Python's basic syntax & data structures.

## Table of Contents

- [Simple With Less "Character Noise"](#simple-with-less-character-noise)

- [If Statements, Functions, Loops, and Significant Whitespace](#if-statements-functions-loops-and-significant-whitespace)

- [Data Structures](#data-structures)

- [Comprehensions](#comprehensions)

[Table of Contents](#table-of-contents)

[Back to README](/README.md)

## Simple With Less "Character Noise"

One of Python's major design goals was to have a simpler, more easily readable syntax. Additionally the desire was for Python to feel more like reading english or pseudo code.

Expressions are heavily inspired by C with some important and meaningful changes / additions

```python
1 + 1
1 - 1
2 * 2
1 / 2  # float division
1 // 2  # integer division
7 % 3
2 ** 3  # exponentiation
2 * (3 + 4)
```

Python tends to have much fewer characters with load-bearing syntactical meaning, especially in sequence. What special characters do exist tend to do this alone in very specific contexts.

Hello World is a single statement

```python
print("Hello, World!")
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## If Statements, Functions, Loops, and Significant Whitespace

Python uses whitespace to signify block structure instead of block delimiting characters such as '{}'.

If statements use `if-elif-else` keywords and do not require parentheses for the expressions being evaluated.

```python
if bit == 0:
    print("bit is 0")
elif bit == 1:
    print("bit is 1")
else:
    print("bit is not a valid binary bit!")
```

For composite expressions the keywords `and` and `or` are used

```python
if a and b or c:
    print("Truthy")
else:
    print("Falsy")
```

### Functions

Functions are defined using the keyword `def`.

```python
def fibonacci(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    
    return fibonacci(n - 1) + fibonacci(n - 2)
```

### Loops

Python supports both for loops and while loops.

Python's for loops differ from the for loops of other languages as they function as what other languages would call a "for-each" loop. They iterate over an iterable rather than using conditionals.

```python
for i in range(5):
    print(i)
```

Python does not support C-style for loops using the triplet of init, condition, and increment.

While loops in Python function as expected.

```python
n = 0
while n < 5:
    print(n)
    n += 1
```

Python does not have a do-while construct.

### Ending statements, semicolons, and backlash

In Python newlines indicate the end of a statement as long as some other construct doesn't remain open such as a set of parentheses or brackets.

Semicolons can be used as optional statement separators in order to place multiple statements on a single line.

```python
a = 1; b = 2
```

As a side effect this means you can optionally end all your lines with a semicolon though both this and putting multiple statements per line are considered bad form and unpythonic.

The backlash character functions as an inverted semicolon which let's you tell the compiler that your statement is not yet finished.

```python
a = 1 + 2 \
    + 3 \
    + 4
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## Data Structures

### Lists

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

### Dictionary

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

### Tuples

Tuples are sequences like lists.

```python
example_tuple = (1, 2, 3, 4)
```

Unlike lists, tuples are immutable.

```python
# raises an exception
example_tuple[0] = 0
```

Due to their immutability, tuples are hashable and can be used to create composite dictionary keys.

```python
composite_key_dict = {(1, "two", 3.0): "value"}
```

### Sets

Python's sets are analogous to sets from mathematics.

```python
example_set = {1, 2, 3, 4}
example_set.add(5)
example_set.add(1)
example_set.remove(2)
```

Sets support mathematical operations for set theory.

```python
s1 & s2  # union
s1 & s2  # intersection
s1 - s2  # difference
s1 ^ s2  # symmetric difference
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## Comprehensions

Python supplies a simple construct for creation the built-in data structures above called comprehensions.

Comprehension syntax is similar to set definition in set theory.

```python
comp_list = [x*2 for x in range(10)]
```

Comprehensions also support conditionals.

```python
odd_numbers_under_20 = [x for x in range(20) if x % 2 == 1]
```

The source of the values in a comprehension can be any iterable type, such as another seq.

```python
comp_list_from_seq = [x*3 for x in [1, 2, 3, 4]]
```

In addition to lists, comprehensions can also generate dictionaries and sets.

```python
comp_dict = {x: x*2 for x in range(10)}
comp_set = {x*2 for x in [1, 1, 2, 2, 3, 3]}
```

Comprehensions can also be nested.

```python
odd_powers_of_3_under_300 = [x for x in [y*3 for y in range(300)] if x % 2 == 1]
```

This style is considered unpythonic as it quickly degrades the readability, and the preference is to split this into two separate comprehensions.

```python
powers_of_3_under_300 = [x*3 for x in range(300)]
odd_powers_of_3_under_300 = [x for x in powers_of_3_under_300 if x % 2 == 1]
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)