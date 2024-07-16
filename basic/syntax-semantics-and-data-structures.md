# Basic Python Syntax & Data Structures

This document introduces Python's basic syntax, semantics, and data structures.

## Table of Contents

- [Simple With Less "Character Noise"](#simple-with-less-character-noise)

- [If Statements, Functions, Loops, Try-Except, With, and Significant Whitespace](#if-statements-functions-loops-and-try-except-with-and-significant-whitespace)

- [Comparison & Arithmetic Operators](#comparison--arithmetic-operators)

- [String Formatting](#string-formatting)

- [Data Structures](#data-structures)

- [Comprehensions](#comprehensions)

- [Unpacking Iterables](#unpacking-iterables)

[Back to Basic Topics](/basic/README.md)

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

## If Statements, Functions, Loops, and Try-Except, With, and Significant Whitespace

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

Python functions also support keyword arguments with defaults which _must_ come after any position arguments.

```python
def foo(pos1, pos2, kw1="default"):
    print(pos1, pos2, kw1)


foo(1, 2)
foo(3, 4, kw1="five")
foo(6, 7, "eight")
```

### Loops

Python supports both for loops and while loops.

Python's for loops differ from the for loops of other languages as they function as what other languages would call a "for-each" loop. They iterate over an iterable rather than using conditionals.

```python
for i in range(5):
    print(i)
```

Python does not support C-style for loops using the triplet of init, condition, and increment.

While loops in Python are very familiar and function as they do in C.

```python
n = 0
while n < 5:
    print(n)
    n += 1
```

Python does not have a do-while construct.

### Try-Except

Python handles errors using exception handling.

```python
try:
    function_that_may_raise_an_error()
except:
    handle_error()
```

You can also catch a specific exception by specifying its type.

```python
try:
    function_that_may_raise_value_error()
except ValueError:
    handle_value_error()
```

If you exception instance is needed, it can be captured.

```python
try:
    function_that_may_raise_value_error()
except ValueError as err:
    handle_value_error(err)
```

You can also specify error handling for multiple kinds of exceptions using multiple `except` blocks.

```python
try:
    function_that_may_raise_several_errors()
except ValueError as err:
    handle_value_error(err)
except TypeError as err:
    handle_type_error(err)
```

If multiple exceptions are handled in the same way, they can be grouped together.

```python
try:
    function_that_may_raise_several_errors()
except (ValueError, TypeError) as err:
    handle_known_errors(err)
```

An optional `else` block can be used to specify code that only runs when _no exception is raised_.

```python
try:
    function_that_may_raise_errors()
except:
    handle_error()
else:
    continue_processing()
```

An optional `finally` block can be used to specify code that _always runs_ after all other triggered blocks are done executing.

```python
try:
    function_that_may_raise_errors()
except:
    handle_error()
else:  # not required to use 'finally'
    continue_processing()
finally:
    do_cleanup()
```

#### Raising Exceptions

Use the `raise` keyword to raise your own exceptions, either with an instance or an exception type.

```python
raise ValueError("error message")
raise TypeError  # implicitly makes an instance
```

You can also "chain" exceptions using `from` so the new exception is "caused" by the original exception and their tracebacks are both displayed.

```python
try:
    raise ValueError("cause")
except ValueError as err:
    raise TypeError("new error") from err
```

Finally, you can re-raise an exception if you need to execute some code due to the error but do not want to suppress it and let it bubble up the call stack.

```python
try:
    raise ValueError
except ValueError:
    do_some_necessary_handling_code()
    raise
```

### Context Management Using `with`

Python supplies a built-in context management block structure using `with` statements.

```python
with some_context_manager():
    do_something_with_activated_context()
```

Context managers "open" when you enter the block, and "close" when it is left -- even if errors occur.

Many context managers return an object for you to use. The following example demonstrates this when contextually opening a file.

```python
with open("path/to/some.file") as infile:
    data = infile.read()
```

You can implement your own context managers that can be used in `with` statements, a topic we will explore in a later training session.

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

## Comparison & Arithmetic Operators

Python borrows it's comparison and arithmetic operators from C.

```python
1 == 1
2 != 1
2 > 1
2 >= 2
1 < 2
1 <= 1

1 + 1
1 - 1
1 * 1
1 / 1
1 // 1
2 ** 2

1 & 3
1 | 3
1 ^ 3
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## String Formatting

Python has 3 distinct ways of formatting strings by interpolating values.

1. f-strings

2. `str.format`

3. `%` formatting

Only f-strings and `%` formatting are syntactic constructs, `str.format` is simply a string method.

This document will not cover `%` formatting as it is an old Python 2 construct and it's use is discouraged. It provides no features beyond what f-strings and `str.format` can accomplish and is far more cumbersome to use.

### f-strings

Python's f-strings provide a simple and intuitive mechanism for interpolating values into strings.

An 'f' prefix is used before a string literal to turn it into a f-string and then variables to interpolate are placed inside '{}' brackets.

```python
name = "Bob"
age = 42

print(f"{name} is {age} years old.")
```

A special feature of f-strings is ending the variable name with an `=`. This causes python to print the variables name and an equal sign as part of the interpolation.

```python
f"{value=}"
```

The interpolated values can be any valid Python expression.

```python
a = 2
b = 3
c = 4
print(f"{a * (b + c)}")
```

### `str.format`

The `format` method of the `str` type is not technically a piece of syntax but is included here for its parallels with f-strings.

`str.format` takes an arbitrary number of positional and/or keyword arguments and interpolates them into a string.

```python
name = "Bob"
age = 42
salary = "800k"

print("{name} is {} years old. {name} has a salary of {1} dollars. {name} only started making {1} dollars a year after he convinced HR to add 'AI' to his job title.".format(age, salary, name=name))
```

While f-strings should be the goto solution when their use is appropriate there are some edge cases where `str.format` should be preferred. A common reason to use `str.format` over f-strings is when you will be formatting a string later but wish to store a template.

```python
person_intro_str_template = "{} is {} years old"

# sometime later
print(person_intro_str_template.format(name, age))
```

### Escaping '{}'

You can escape the interpolating brackets in f-strings and strings you format using `str.format` by uing pairs of same brackets.

```python
bracket_name = "squiggly"

print(f"open {bracket_name} brackets are {{")
print(f"closing {bracket_name} brackets are }}")
```

### Format Specifiers

Format specifiers are using when formatting strings to adjust the formatting and can provide features like limiting floating point precision, justification, and changing fill characters.

In f-strings they come after variable names, when using `str.format` they are just the contents of the brackets.

```python
# Interpolating a float with the features:
#   - field width 8
#   - right justified
#   - '0' as fill character
#   - 3 digits after decimal

float_value = 42.1337

print(f"{float_value:0>8.3f}")
print("{:0>8.3f}".format(float_value))
```

A quick reference resource for format specifiers can be found at [pyformat.info](https://pyformat.info). For complete but less digestible documentation of format specifiers you can read [PEP 3101 - Advanced String Formatting](https://peps.python.org/pep-3101/).

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## Data Structures

The following built-in data structures are included in this syntax document as Python has special syntax for instantiation instances of these types.

### Lists

Lists are mutable sequences, analogous to arrays in C, which can be instantiated using square brackets

```python
[1, 2, 3, 4]
```

### Dictionary

Dictionaries are key-value hash maps which use curly brackets and colons for literals.

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

### Tuples

Tuples are immutable sequences which can be instantiated used parentheses.

```python
example_tuple = (1, 2, 3, 4)
```

In areas of code that lack ambiguity, the parentheses are optional.

```python
example_tuple = 1, 2, 3, 4
```

### Sets

Sets are hash sets that use curly brackets for literals.

```python
example_set = {1, 2, 3, 4}
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

## Unpacking Iterables

An important feature of Python is being able to unpack an iterable of N elements into N variables in a single statement. In essence you assign a tuple of _variable names_ to an iterable and the values are unpacked in order.

```python
example_list = [1, 2, 3]
first, second, third = example_list
```

Like tuples, you can also use parentheses.

```python
example_list = [1, 2, 3]
(first, second, third) = example_list
```

This feature is used heavily with for loops.

```python
people = [  # a list of tuples
    ("Bob", 30),
    ("Sarah", 40),
    ("John", 50),
]

for name, age in people:
    print(f"{name} is {age} years old")
```

There are also mechanisms for handling unknown numbers of elements that will be explored later.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)
