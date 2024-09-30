# Tooling

This document covers some useful tools for tooling your Python projects.

## Table of Contents

- [Managing Python Versions - `pyenv` & The `py` Launcher](#managing-python-versions---pyenv--the-py-launcher)

- [REPL - `iPython`](#repl---ipython)

- [Project Management - `poetry`](#project-management---poetry)

- [Code Formatting - `black`](#code-formatting---black)

- [Static Type Checking - `mypy`](#static-type-checking---mypy)

- [Testing - `pytest`](#testing---pytest)

- [Linting - `pylint`](#linting---pylint)

- [`pre-commit`](#pre-commit)

- [Import Sorting - `isort`](#import-sorting---isort)

[Back to Advanced Topics](/advanced/README.md)

## Managing Python Versions - [`pyenv`](https://github.com/pyenv/pyenv) & The `py` Launcher

`pyenv` is a Python toolchain multiplexer for linux environments. It can manage install and manage multiple versions and provides excellent tools for deciding which version to use.

```bash
# install python 3.12.6
pyenv install 3.12.6

# select 3.12.6 globally
pyenv global 3.12.6

# select 3.11.5 in the current dir (and subdirs)
pyenv local 3.11.5

# display installed versions
pyenv versions

# list available versions to install
pyenv install --list
```

The `py` launcher is an executable for Windows for managing Python versions. While not as robust as `pyenv` it serves its purpose well for allowing you to run against specific Python versions.

```powershell
# run the default python version
py

# run a script with the default python version
py my_script.py

# run 3.12
py -3.12

# list available versions
py --list
```

The `py` launcher does not support multiple patch versions of the same minor version of Python.

It is installed along with Python using the Windows installers, though you must ensure the feature is enabled in the installer.

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## REPL - [`iPython`](https://ipython.org)

`iPython` is a advanced REPL (Read-Eval-Print-Loop) for Python and is intended to replace using `python` as an interpreter.

It supports many useful features such as command history, multiple lines, auto-completion, advanced help, and direct access to the terminal.

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## Project Management - [`poetry`](https://python-poetry.org)

`poetry` is a project management tool for Python. It manages dependencies, project configuration, and tool configuration by way of the "pyproject.toml" file.

It automatically manages a virtual environment for your project.

```bash
# add a dependency to the main dependencies
poetry add some-dep

# add a dependency to the `dev` dependency group
poetry add --group=dev some-dev-dep

# remove a dependency from the main deps
poetry remove unneeded-dep

# show all deps
poetry show

# show all deps in a tree format
poetry show --tree

# run something in the virtual env
poetry run some_command

# enter a shell with the virtual env enabled
poetry shell
```

The "pyproject.toml" file is not unique to `poetry` and is in fact a standard adopted for Python via the PEP system. `poetry` is simply an implementer of this feature.

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## Code Formatting - [`black`](https://black.readthedocs.io/en/stable/)

`black` is a code formatter for Python which made the unique decision to support almost zero configuration. This reduces bikeshedding and makes all black formatted code look exactly the same.

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## Static Type Checking - [`mypy`](https://mypy-lang.org)

`mypy` is a static type checker for Python, written by Guido, which reads and evaluates type annotations to ensure type correctness of your application.

It is heavily configurable and includes extensions to the type system which supports extended typing features.

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## Testing - [`pytest`](https://docs.pytest.org/en/stable/)

`pytest` is a powerful testing framework for Python. Some of it's most notable features are as follows:

1. Scoped testing fixtures

    ```python
    import os
    from collections.abc import Iterator
    from unittest import mock

    import pytest


    @pytest.fixture(scope="function")
    def mock_database() -> Iterator[mock.MagicMock]:
        with mock.patch("some_db_lib.Database") as mock_db:
            yield mock_db

    
    @pytest.fixture(scope="session", autouse=True)
    def mock_environment() -> Iterator[None]:
        patch = {
            "APP_NAME": "local-testing",
            "DB_URI": "sqlite://",
        }
        with mock.patch.dict(os.environ, patch):
            yield

    @pytest.fixture()
    def value_fixture() -> str:
        return "some_value"

    @pytest.fixture()
    def fixture_with_cleanup() -> None:
        # do some setup
        yield  # tests run
        # do some cleanup
    ```

2. Use of "assert"

    `pytest` catches Python's `AssertionError` and uses stack frame manipulation to provide expanded results including diffs for failing assertions.

    In tests you use `assert` directly instead of needing to import a special pytest specific assert.

    ```python
    def test_some_value():
        assert SOME_VALUE == "some_value"
    ```

3. Powerful test configuring

    `pytest` uses reads files called `conftest.py` within testing directories, to configure the tests in that folder.

    All `conftest.py` files in parent directories are also used allowing for powerful nested configurations and isolation of global fixtures.

4. Expansive library of extensions

    `pytest`'s power and popularity have caused an ecosystem to be built around in with numerous useful extension libraries supporting anything from coverage, json reports, parallel test execution, and beyond.

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## Linting - [`pylint`](https://pylint.readthedocs.io/en/stable/)

`pylint` is a heavily configurable linter for Python and is a common goto for experienced Python developers due to its vast array of checks and heavy allowance for configuration to meet the needs of your project.

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## [`pre-commit`](https://pre-commit.com)

`pre-commit` allows you create declarative git pre-commit hooks.

First specify your hooks in `pre-commit-config.yaml` at the root of your application repo.

```yaml
# pre-commit-config.yaml
default_language:
    python: python3.12

repos:
    # define a hook provided by another tool
    - repo: https://github.com/psf/black
      rev: 24.8.0
      hooks:
        - id: black
          language_version: python3.12

    # define a custom hook running from the local repo
    - repo: local
      hooks:
        - id: my-cool-hook
          name: my-cool-hook-display-name
          entry: poetry run python hooks/my_cool_hook.py
          language: system
```

Then install the hooks using pre-commit:

```bash
pre-commit install
```

While `pre-commit` is written in Python, it is often used for other languages.

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## [Import Sorting - `isort`](https://pycqa.github.io/isort/)

`isort` sorts the imports in your module in a consistent way which helps reduce the cognitive load on developers when parsing imports. Because `isort` sorts the imports into blocks -- 1st party, 3rd party, and local -- and then sorts each block alphabetically it is easy for developers to determine what is imported and from where.

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)