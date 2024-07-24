## Project Structure & Import Mechanics

This document introduces the standard structure for Python projects as well as Python's import mechanics and their ramifications.

## Table of Contents

- [Import Mechanics](#import-mechanics)

- [Modules vs Packages](#modules-vs-packages)

- [Import Statements](#import-statements)

- [Project Structure](#project-structure)

[Back to Intermediate Topics](/intermediate/README.md)

## Import Mechanics

This section will describe Python's import mechanisms as well as how to use and select your import style.

Python's import system uses modules and packages, which are singular files and collections of other modules / packages respectively.

Full documentation on the import system can be found [here](https://docs.python.org/3/reference/import.html)

### Import Means Execution

When a module is imported it is executed _as if it was a script_.

Consider the following example:

```python
# example.py

def foo():
    print("bar!")

print("baz!")
```

If you import this module it will immediately print "baz!" as the module was executed as part of the import.

### No Recursive Imports

Python does not support recursive imports. When a module is executing due to being imported or executed and an `import` statement is encountered, execution is paused in order to import and execute the module / package being imported.

If at any time in this execution chain, a module or package already in the import chain is imported an error will occur as there is no way to finish execution of any of the modules or packages since they rely on each other. 

Consider the follow 2 modules:

```python
# a.py
import b

A_GLOBAL = "VALUE"

# b.py
import a

B_GLOBAL = a.A_GLOBAL.lower()
```

When you import either module, you will receive an import error due to the recursion.

This issue is not just in direct recursion, but in indirect recursion.

Care should be taken when structuring your project to prevent this from happening. 

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Modules vs Packages

A "module" is simply a `.py` file, while a package is directory with at least the special file `__init__.py` at its root. A package can contain an arbitrary number of other modules and packages within it, but the existence of the `__init__.py` file is what makes a directory a package.

The `__init__.py` file is a special root module of a package, and when you import a package using just the package's name (remember: it's a directory!), the `__init__.py` file is what is imported and executed.

`__init__.py` files are often empty, and you should expect many empty `__init__.py` files in your projects.

A common use case for non-empty `__init__.py` is to collect imports from the package into a singular place to create a developer friendly import interface into a library's functionality.

```
example_module.py

example_package/
|
+-- __init__.py
|
+-- sub_module_1.py
|
+-- sub_module_2.py
|
+-- sub_package/
    |
    +-- __init__.py
    |
    +-- sub_sub_module.py
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Import Statements

This section will cover absolute vs relative imports, the two types of import statements, "star" importing, as well as when and how to use them.

### Package / Module Imports

Package and module imports import the namespace of a package or module into the current module.

```python
import module
import package
```

You can then access the members of the namespace using the `.` operator.

```python
module.member
package.member
```

If you are importing a nested module or package then you provide the full package / module path separated by periods.

```python
import a.nested.module
import a.nested.package
```

### Member Imports

Member imports allow you to pull specific members from the namespace of a module or package into the _current_ namespace, unqualified.

```python
from module import module_member, another_module_member
from package import package_member, another_package_member

from a.nested.module import nested_module_member, another_nested_module_member
from a.nested.package import nested_package_member, another_nested_package_member
```

### Absolute Imports

The path to a particular package or module is based on the directory of the root module being executed.

A root module inside a package uses the _top level package_ as it's root path for import lookup.

Consider the following import statement and file system:

```python
import a.b.c
```

```
cwd/
|
+-- entry.py
|
+-- package/
|   |
|   +-- __init__.py
|   |
|   +-- entry.py
|
+-- folder/
|   |
|   +-- entry.py
|
+-- a/
    |
    +-- __init__.py
    |
    +-- b/
        |
        +-- __init__.py
        |
        +-- c.py
```

Assume you are in `cwd/` and you execute `entry.py` which contains the import statement:

```bash
python entry.py
```


Lookup starts from `cwd/` since that is the folder that contains the root script `entry.py` and the import statement will succeed.

If you execute `package/entry.py`:

```bash
python package/entry.py
```

Then the import statement will still succeed since `package/` is a package due to the `__init__.py` file and thus lookup still starts inside `cwd/`.

However, if you execute `folder/entry.py`:

```bash
python folder/entry.py
```

Then the import statement will fail since the import lookup will start _from inside folder/_.

### Relative Imports

If you start the path to a module or package with one or more leading periods then Python will perform a relative import.

Relative imports _only support member imports_.

Relative imports are relative to the _module containing the import statement_ and only _within the containing top level package_.

A single period specifies the current package containing the module, and each extra leading period signifies stepping up one level.

Consider the following file system:

```
cwd/
|
+-- first/
    |
    +-- __init__.py
    |
    +-- target.py
    |
    +-- second/
        |
        +-- __init__.py
        |
        +-- target.py
        |
        +-- third/
            |
            +-- __init__.py
            |
            +-- target.py
            |
            +-- main.py
```

If the import statement were in `main.py` then you would encounter the following behavior:

```python
from .target import value #  imports from first/second/third/target.py

from ..target import value # imports from first/second/target.py

from ...target import value # imports from first/target.py

from ....target import value # raises an ImportError because you are beyond the top level package
```

Python does not see or understand packages above the root execution directory of a module. This means that relative imports will not function correctly if they are within the root script you are executing and you try and import relatively from a parent of that scripts folder.

Because relative imports make it hard to see the structure of your project from a particular file the Python community heavily favors absolute imports except in cases where relative imports are the only way.

One particular use case for relative imports will be described later in this document.

### Star Imports

A asterisk -- "*" -- can be used in place of any member names in member import to import _everything_ from that package / module into the namespace of the importer.

```python
from target import *
```

This degrades readability by convoluting where something was imported from within a file, and additionally can cause name shadowing if name collisions occur. In the case of name shadowing the _last imported members with the name will override all the other members_.

For these reasons using "star" imports at all is considered bad practice and should not be done in your packages or modules.

The use of star imports is when you are inside Python's REPL where it can be a convenient shortcut to import all the members in your target module or package.

### Overriding Imported Name

If you need to override the name of an imported member you can do so using `as`.

```python
import module as other_module
from package import member as other_member, second_member as different_member
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)

## Project Structure

This section seeks to describe a standard project structure as accepted by the Python community and expected by much of the tooling that will be described later.

### Source Code

The project's source code should be in a module _or_  package at the top level of the repository that shares it's name with the project, using underscores over hyphens for whitespace.

```
my_cool_module_project.py
```

_OR_

```
my_cool_package_project/
|
+-- __init__.py
|
+-- ...
```

### Test Code

Tests should be places at the root of the repository in a folder called `tests/`.

To aid in test discovery by test frameworks and to help with importing test utilities the tests folder should be a package.

```
tests/
|
+-- __init__.py
|
+-- unit/
|   |
|   +-- __init__.py
|   |
|   +-- ...
|
+-- integration
|   |
|   +-- __init__.py
|   |
|   +-- ...
|
+-- ...
```

The Pythonic convention for unit test files is to exactly match the structure of the source code package, with each test module -- the `.py` files -- having a `test_` prefix. Python's test frameworks rely on the prefix for discovering the appropriate files.

```
my_cool_project/
|
+-- __init__.py
|
+-- my_cool_module.py
|
+-- my_cool_subpackage/
|   |
|   +-- __init__.py
|   |
|   +-- my_other_cool_module.py
|
tests/
|
+-- __init__.py
|
+-- unit/
|   |
|   +-- __init__.py
|   |
|   +-- test___init__.py
|   |
|   +-- test_my_cool_module.py
|   |
|   +-- my_cool_subpackage/
|       |
|       +-- __init__.py
|       |
|       +-- test___init__.py
|       |
|       +-- test_my_other_cool_module.py
|
+-- ...
```

If you choose to split your tests into their own root folders, then unit tests should be placed directly under `tests/`.

```
tests/
|
+-- acceptance_tests/
|
+-- integration_test/
|
+-- ...
```

[Table of Contents](#table-of-contents)

[Back to Intermediate Topics](/intermediate/README.md)
