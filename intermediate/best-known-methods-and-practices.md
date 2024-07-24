# Best Known Methods & Practices

This document explains some of the best known methods and practices when using Python.

## Table of Contents

- [Virtual Environments](#virtual-environments)

- [Multi-Paradigm](#multi-paradigm)

- [Prefer Simple Comprehensions](#prefer-simple-comprehensions)

- [Prefer Context Managers](#prefer-context-managers)

- [Use the Ecosystem](#use-the-ecosystem)

- [REPL Driven Development](#repl-driven-development)

[Back to Intermediate Topics](/intermediate/README.md)

## Virtual Environments

Using your global Python environment as a staging area for projects is prone to conflict as multiple projects demand different dependencies.

Using a virtual environment -- an isolated environment with its own tooling and dependencies -- is the best known method for managing the development environment of a project.

Python has a built in module for generating virtual environments called `venv`. To create a new virtual environment, you invoke the module from Python.

```bash
python -m venv path/to/new/virtual/environment
```

Once created, it must be activated.

Linux:

```bash
source <venv-path>/bin/activate
```

The invoked script may change depending on your shell.

Windows:

```powershell
<venv-path>\Scripts\activate.ps1
<venv-path>\Scripts\activate.bat
```

While active the "python" command will invoke the virtual environments interpreter, and "pip" will install into the virtual environment.

You can read more about `venv` by reading its [documentation](https://docs.python.org/3/library/venv.html).

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Multi-Paradigm

When creating scripts or applications in Python you should use the programming paradigm that you feel most clearly maps your problem space.

New Python programmers commonly apply object oriented programming exclusively due to everything in Python being an object. While this is true, it does not enforce OO practices, classes are just the way custom types are created in Python.

Between Python's powerful class definition system, first class functions, and functional built-ins, comprehensions, and modules your app should be structured around which paradigm most clearly expresses the intent of each individual piece of functionality.

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Prefer Simple Comprehensions

Comprehensions should always be preferred over for loops or functions like `map` when the logical complexity is low.

```python
from pathlib import Path

# do this
json_files = [filepath for filepath in Path.cwd().glob("*.json") if filepath.isfile()]

# don't do this
json_files: list[Path] = []
for filepath in Path.cwd().glob("*.json"):
    if filepath.isfile():
        json_files.append(filepath)
```

Some reasons to avoid using a comprehension:

- Heavy processing required for the value

- Very complex conditional logic

- Processing steps change based on conditionals

Avoid using nested comprehensions.

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Prefer Context Managers

You should prefer using context managers for types that implement it. This ensures appropriate cleanup occurs even when errors occur.

```python
# do this
with open("my.file") as infile:
    data = infile.read()

# don't do this
infile = open("my.file")
data = infile.read()
infile.close()
```

Additionally consider implementing your own types as context managers when it is a sensible decision.

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Use the Ecosystem

Python has a rich ecosystem of 3rd party libraries. Many of them, especially the popular ones, have excellent documentation and a wealth of examples and discussion about their use online.

Unless you have a very compelling reason to do so, avoid reinventing the wheel and instead research a library that provides the functionality you need so you are free to focus on your products unique logic.

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## REPL Driven Development

Leverage Python's Read-Eval-Print-Loop during the exploratory portion of your projects. Especially with more a more powerful REPL like `iPython`.

When exploring the details of an implementation or even just toying with the functionality of a new library to see the specifics of how it works, the REPL provides incredibly fast iteration and the ability to easily change parameters during execution much like a debugger.

Additionally you will receive additional practice which solidifies the use of Python's build-ins, standard library, and language features until they become second nature.

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)
