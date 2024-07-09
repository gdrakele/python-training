# Introduction to Python as a Technology

This document serves as a basic introduction to Python and its goals, ideals, and uses.

## Table of Contents

- [Origin and History](#origin-and-history)

- [As a Language](#as-a-language)

- [Python Enhancement Proposals - PEPs](#python-enhancement-proposals---peps)

- [PIP & PyPI](#pip--pypi)

- [The Zen of Python](#the-zen-of-python)

[Back to Basic Topics](/basic/README.md)

## Origin and History

- Designed by Guido van Rossum

    - Focus on the language being easy and intuitive

    - Syntax should be easy to understand

    - Language must be powerful and complete

    - Open source so anyone can contribute

- Released in February 1991

    - Python 2 in October 2000 / EOL January 2020

    - Python 3 in December 2008

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## As a Language

- Useful for scripting all the way to applications

    - Excellent for quick scripts and automation

    - Robust and powerful enough for complete production applications

    - Single language for all your product's needs

        - Project source

        - Automation

        - Scripts

- Robust standard library, "batteries included language"

- High level programming in C

    - CPython is written in C

    - Python is what you get when you take "you can do that in C" to the extreme

- Performance is slow, but also fast

    - Pure Python can be quite slow compared to other languages - 40x slower than C

    - Many highly optimized libraries written at a low level in languages like C, C++, or Fortran

    - Provides "glue" between these libraries with vastly improved developer ergonomics

    - C extensions, Cython, and Numba can provide performance where you need it in bespoke code.

- Ease of exploratory coding

    - Prototypes and PoCs can be developed very quickly

    - Iterating to reach that "perfect implementation" is extremely fast

    - Python APIs tend to be simple and intuitive while maintaining completeness of functionality

    - The ecosystem tends to have excellent documentation

- Selecting Python as your language

    - When to use Python

        - Cloud applications like web frameworks

        - Automation tasks and scripts

        - Machine Learning

        - Libraries w/o heavy performance requirements

            - Only when using Pure Python

            - Providing C extensions or equivalent for the performance critical pieces can produce highly performant libraries

    - When to reconsider using Python

        - Libraries w/ heavy performance requirements

            - See above for caveats

        - Client applications*

            - Requires Python on-system

            - Project Freezing Applications - Pyinstaller & Nuitka

            - Large binary sizes

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## [Python Enhancement Proposals - PEPs](https://peps.python.org)

- Primary mechanism for improving Python

    - Fosters discussion in the community and amongst the core maintainers

    - Provides changelogs of proposals over time

- Anyone can contribute

- Not guaranteed to be accepted

    - Remain in "rejected" state

    - New PEPs with similar goals can be accepted in the future

    - Allows for future PEP authors to invent solutions to problems that caused rejection

- Upcoming features can be viewed

    - Includes version planned to contain the enhancement

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## PIP & PyPI

- [PIP Installs Packages](https://pypi.org/project/pip/)

    - Package installer for Python

    - Built-in to installations or Python (usually)

        - Sometimes you must jump through hoops to get PIP on a system or to work as expected

    - Lacking features of a dependency management system

    - Dependency specification from requirements.txt

        ```
        requests==1.0.0
        fastapi>=2.0.0
        sqlalchemy>2.1.1,<3.0.0
        ```

- [Python Package Index](https://pypi.org)

    - Location for 3rd party packages

    - Software is available to host an internal PyPI

    - PIP can have its indexes redirected and/or extended with precedence declarations if required

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/basic/README.md)

## The Zen of Python

A humorous distillation of the language goals and ideals of Python.

```
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

[Table of Contents](#table-of-contents)

[Back to Basic Topics](/README.md)