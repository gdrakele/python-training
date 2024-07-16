# Differences to Other Languages

This document records some of Python's differences as compared to other common languages.

## Table of Contents

- [Lack of Compile Time Type Checking](#lack-of-compile-time-type-checking)

- [Variable Scope](#variable-scope)

- [No Function Overloading](#no-function-overloading)

- [Assignment Is Not an Expression](#assignment-is-not-an-expression)

[Back to Basic Topics](/basic/README.md)

## Lack of Compile Time Type Checking

Python does not check types before running. Because Python is strongly typed, this means that nonsensical statements will be not caught prior to execution and will instead raise an exception when executed.

```python
def this_function_will_always_raise_an_error_when_called():
    return 1 + "1"
```

This also means that bad input to an otherwise fine function will cause an error.

```python
def double(num):
    return num * 2

double("2")
```

This is especially dangerous when accepting external input.

These issues can be easily solved with tooling and libraries as will be described in later training sessions.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## Variable Scope

Python's variable scoping rules have some important differences from other languages. One of the most important is that if-statements do not create a scope, variables assigned within an if statement can be accessed outside of that statement.

```python
if some_condition
    name = "Bob"
print(name)
```

Care must be taken to ensure all branches define a variable or an exception will be raised during execution. A later training session will discuss tooling that will catch this potential problem before runtime.

Python also handles global variables and nonlocal variables -- that is, variables captured in closures -- in a novel way to make mutation of such variables more explicit. If you only _read_ from a global or nonlocal variable within a lower scope, you only need the variable.

```python
EXAMPLE_GLOBAL = "global"

def foo():
    print(EXAMPLE_GLOBAL)

def closure_factory():
    example_nonlocal = "nonlocal"
    
    def closure():
        print(example_nonlocal)
    
    return closure
```

If you assign to a global or nonlocal variable in such contexts, instead a local variable shadowing the name will be created.

```python
EXAMPLE_GLOBAL = "global"

def foo():
    EXAMPLE_GLOBAL = "GLOBAL"
    print(EXAMPLE_GLOBAL)  # prints 'GLOBAL'

print(EXAMPLE_GLOBAL)  # prints 'global'

def closure_factory():
    example_nonlocal = "nonlocal"
    
    def closure(new_value):
        example_nonlocal = "NONLOCAL"
        print(example_nonlocal)

    closure()  # prints 'NONLOCAL'
    print(example_nonlocal)  # prints 'nonlocal'
```

You must explicit declare variable global or nonlocal using the `global` and `nonlocal` keywords respectively when you plan to assign to them within a lower scope.

```python
EXAMPLE_GLOBAL = "global"


def foo():
    global EXAMPLE_GLOBAL
    EXAMPLE_GLOBAL = "GLOBAL"
    print(EXAMPLE_GLOBAL)


def closure_factory():
    example_nonlocal = "nonlocal"

    def closure():
        nonlocal example_nonlocal
        example_nonlocal = "NONLOCAL"
        print(example_nonlocal)

    return closure
```

Specifically `global` tells Python to use a variable defined at global module scope, and `nonlocal` tells Python to walk up scope context until it finds the variable. `nonlocal` cannot be used to reference a global variable.

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## No Function Overloading

Python does not support function overloading. If you try to overload a function then _the last defined_ function will take precedence.

```python
def foo():
    print("Hi!")

def foo():
    print("Hello!")


foo()  # prints 'Hello!'
```

Through the use of tooling you can get warnings about such name shadowing.

If you desire similar functionality from Python, the preferred method is to provide multiple functions with descriptive names.

```python
def make_int_from_string(string):
    return int(string)

def make_int_from_file(filepath):
    with open(filepath) as infile:
        return int(infile.read())
```

You could also use keyword arguments as a "switch", though this is considered bad form and should be saved for adjusting functionality rather than wholly replacing it.

```python
# don't do this
def make_int(source, source_type="string"):
    if source_type == "string":
        return int(string)
    elif source_type == "file":
        with open(filepath) as infile:
            return int(infile.read())
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## Assignment Is Not an Expression

Assignment in Python is not an expression and is instead a statement. This is done to protect you from accidently using `=` when you meant `==` in certain contexts.

```python
condition = False
if condition = True:  # raises a compile time error
    ...
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)
