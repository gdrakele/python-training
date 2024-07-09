# Basic

- Python as a Technology

    - Why is it called Python

    - When Python was released

    - Scripting to Applications

    - "high level programming in C"

    - Ease of "exploratory coding"

    - Slow, but fast

        - "Glue" for low level libraries

        - Improved developer ergonomics

    - Explanation of PEPs

    - The Zen of Python

    - When to use Python?

- Basic Syntax

    - Simple, less "character noise"

        - "Hello World" example

    - Block structures and whitespace is significant

        - if statements

        - Functions

            - Fibonacci

        - Loops

        - Context managers

    - Newlines end statements, backslash is "inverted semicolon"

- Differences from other common languages

    - Variable scope

    - Globals / Nonlocals

    - No function overloading

        - Use different functions or kwargs

    - Assignment is not an expression

- Built-in types and functions

# Intermediate

- Intermediate syntax

    - yield

    - Unpacking with `*` and `**`

    - `else` in `for`, `while`, and `try`

    - Walrus operator `:=`

    - async

- Project structure, import mechanics, "main", and special filenames

    - Imports cause execution

    - No recursive imports

    - Types of imports

        - package / module imports - `import x`

        - object imports - `from x import y`

        - relative imports - `from ..x import y`

        - star imports - `from x import *`
    
    - Packages and `__init__.py`

    - `__main__.py` as an entry point

- Magic methods and the basics of the Python data model

    - Comparison magic methods

    - Overriding / hooking into the built-in functions / statements

    - `a + b` is `a.__add__(b)`

- Basic Object-Oriented Programming

    - Essentially a namespace

    - Class bodies are executed

    - The initializer `__init__`

    - Explicit "self", bound methods, and the implications

- Type Annotations

    - TypeVars & Generics

    - Unions

    - Type aliases

    - Protocols

- BKMs

    - Virtual Environments

    - Multi-paradigm
    
    - Comprehensions

    - Context managers

    - Using the ecosystem

    - REPL driven development

- Common gotchas

    - Mutable default values for parameters

    - Recursion limits

- Intermediate Data Model

    - context managers with `__enter__` and `__exit__`

    - iterators with `__iter__`

# Advanced

- Tooling

    - Managing Versions - Pyenv and the Py Launcher

    - REPL - iPython

    - Project Management - Poetry

    - Code Formatting - Black

    - Static Type Checking - MyPy

    - Testing - Pytest

        - Testing BKMs

            - Mocks via `unittest.mock`

            - Fixtures

            - Asserts

            - Conftest

    - Linting - Pylint

    - Pre-commit?

    - Import Sorting - ISort

- Advanced Data Model

    - `type` is an instance of `object`, `object` is of type `type`

    - `__new__`

        - Singleton example

    - Tying `type(...)` to `class ...:`

    - the "right" comparison operators

    - Metaclasses

- Notable libraries

    - Standard Library

        - functools

        - itertools

        - dataclasses

        - enum

        - abc

        - typing

        - collections.abc

        - \_\_future__

    - 3rd party

        - litestar / fastapi

        - pydantic / pydantic-settings

        - polyfactory

        - pendulum

        - sqlalchemy

        - loguru

        - invoke

        - typer / click

        - type stubs

        - boto3

        - Pyinstaller / Nuitka

# Backlog

- Installing Python

- Writing docstrings

- Pythonic examples

    - Open a file for read / write

    - Globbing a file tree

    - Walk a file tree recursively

- Gotchas about Types (strings are immutable/interned, etc)

- Differences
    
    - Lack of compile time type checking
