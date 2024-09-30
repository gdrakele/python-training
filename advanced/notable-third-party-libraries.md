# Notable 3rd Party Libraries

## Table of Contents

- [`pydantic`](#pydantic)

- [`pendulum`](#pendulum)

- [`loguru`](#loguru)

- [`requests`](#requests)

- [`typer`](#typer)

- [`sqlalchemy`](#sqlalchemy)

- [`litestar`](#litestar)

[Back to Advanced Topics](/advanced/README.md)

## [`pydantic`](https://docs.pydantic.dev/latest/)

`pydantic` is a library very similar to dataclasses but with a lot of enhanced functionality, the primary of which is data validation.

Where `dataclasses` allows automatic generation of classes using type annotations, you can still assign incorrect types to parameters. `pydantic` validates the incoming data ensuring you have the types you expect and provides excellent, descriptive error messages if the data validation fails.

```python
import pydantic


class Person(pydantic.BaseModel):
    name: str
    age: int


# you must use keyword arguments for initialization
Person(name="Bob", age=42)

# it will coerce compatible types to the type you expect
assert Person(name="Sarah", age="33").age == 33
```

It also provides heavy (de)serialization capabilities.

```python
person = Person.model_validate(
    {
        "name": "Steve",
        "age": 27
    }
)
assert person == Person(name="Steve", age=27)

person_dict = Person(name="Lily", age=38).model_dump()
assert person_dict == {"name": "Lily", "age": 38}
```

It supports aliasing fields which can help with keyword shadowing or exposing names to the outside worlds such as when, for example, building a model from an incoming JSON web request.

```python
from typing import Annotated


class Error(pydantic.BaseModel):
    type_: Annotated[str, pydantic.Field(alias="type")]
    message: str
```

It supports many of Python's built-in types in painless and intuitive ways.

```python
import enum


class Suit(enum.StrEnum):
    SPADES = enum.auto()
    CLUBS = enum.auto()
    HEARTS = enum.auto()
    DIAMONDS = enum.auto()


class Rank(enum.StrEnum):
    ACE = enum.auto()
    KING = enum.auto()
    QUEEN  = enum.auto()
    JACK = enum.auto()
    TEN = enum.auto()
    NINE = enum.auto()
    EIGHT = enum.auto()
    SEVEN = enum.auto()
    SIX = enum.auto()
    FIVE = enum.auto()
    FOUR = enum.auto()
    THREE = enum.auto()
    TWO = enum.auto()


class Card(pydantic.BaseModel):
    suit: Suit
    rank: Rank


card = Card.model_validate(
    {
        "suit": "spades",
        "rank": "ace",
    }
)
assert card.suit is Suit.SPADES
assert card.rank is Rank.ACE
```

It also supports custom validators which can be used for your own custom validation logic or even to provide pre-processing.

```python
from typing import Any
def lowercase_if_str(value: Any) -> Any:
    if isinstance(value, str):
        return value.lower()

    return value


class Card(pydantic.BaseModel):
    suit: Annotated[Suit, pydantic.BeforeValidator(lowercase_if_str)]
    rank: Annotated[Suit, pydantic.BeforeValidator(lowercase_if_str)]


card = Card.model_validate(
    {
        "suit": "SPADES",
        "rank": "QuEeN",
    }
)
assert card == Card(suit=Suit.SPADES, rank=Rank.QUEEN)
```

While this is just a short introduction to some of Pydantic's functionality, it is an incredibly deep and powerful library. Explore its excellent documentation to see everything it can do!

When deciding between using `pydantic` or using `dataclasses` due to their incredibly similar functionality, I use the following rules:

1. If the type is an internal type, use `dataclasses`

2. If the type interacts with the "outside world" via reading input and/or sending output, use `pydantic`.

This is due to the performance overhead of the validation stages. When you must read input from the outside such validation is vital, and when you are sending data back out the robust serialization functionality is extremely useful.

### `pydantic-settings`

`pydantic` also supports application configuration by way of the `pydantic-settings` extension. Models will load and validate values from the secrets, environment variables, and more.

```python
from pydantic_settings import BaseSettings


class AppSettings(BaseSettings):
    app_name: str
    db_uri: str
    db_name: str


# will read envvars: APP_NAME, DB_URI, DB_NAME
settings = AppSettings()
```

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## [`pendulum`](https://pendulum.eustace.io)

`pendulum` is a datetime library with a few very important advantages over Python's built-in `datetime`.

1. `pendulum.DateTime` is timezone aware _by default_ as opposed to `datetime.datetime` being naive by default.

2. Extremely intuitive and complete API.

    ```python
    now = pendulum.now()  # computer's timezone

    utc_now = pendulum.now("UTC")

    today_at_noon = now.at(hour=12)

    interval_between_now_and_noon = today_at_noon - now

    time_between_now_and_noon = interval_between_now_and_noon.as_duration()

    thirty_minutes_from_now = now.add(minutes=30)
    ```

3. `pendulum`'s types all inherit from `datetime`'s types so they are largely interoperable with the rest of Python.

I highly recommend using `pendulum` in all your projects that must interact with dates and times for the ease of use and protection against accidentally omitting timezones.

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## [`loguru`](https://loguru.readthedocs.io/en/stable/)

`loguru` is logging library which focuses both on heavily streamlining the process of configuring the logger and providing a feature rich system for producing exactly the logs you wish to see. One of it's primary goals is to get out of your way, and to that end it comes pre-configured with sane out-of-the-box defaults.

You can begin logging with loguru as simple as the following:

```python
from loguru import logger


logger.info("info level log")
```

`loguru.logger` is a singleton logger and the only logger you will ever use. Any part of your project that needs to write logs simply imports the logger from `loguru` and begins using it.

Full control over adding log sinks and their configuration is provided via the `logger.add` function.

```python
import sys

# output serialized (JSON structured) logs to stdout
logger.add(sys.stdout, serialize=True)

# output logs to a file
logger.add("logs.txt")
```

You can also contextualize logs which provides much enhanced traceability.

```python
with logger.contextualize(user_id="some_user"):
    ...  # all logs within the context will have `user_id` as part of the record
```

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## [`requests`](https://requests.readthedocs.io/en/latest/)

`requests` is a library for performing web requests and boasts an extremely friendly API and robust and complete functionality.

```python
import requests
from http import HTTPStatus


response = requests.get("https://my.cool.api")
assert response.status_code == HTTPStatus.OK
print(response.json())

payload = {"name": "Bob", "age": 42}
response = requests.post("https://my.cool.api/person", json=payload)
assert response == HTTPStatus.CREATED
```

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## [`typer`](https://typer.tiangolo.com)

`typer` is a library for writing CLIs using type annotations in much the same way as `pydantic`.

```python
# cli_example.py

from typer import Typer


cli = Typer()


@cli.command()
def greet(name: str, be_friendly=True) -> None:
    if be_friendly:
        print(f"Hello, {name}! Good to see you again!")
    else:
        print(f"Oh, it's you again, {name}. Just my luck...")


@cli.command()
def farewell(name: str) -> None:
    print(f"Goodbye, {name}!")


if __name__ == "__main__":
    cli()
```

You could now run this CLI as follows:

```bash
python cli_example.py greet Bob
python cli_example.py greet Steve --no-be-friendly
python cli_example.py farewell Alice
```

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## [`sqlalchemy`](https://www.sqlalchemy.org)

`sqlalchemy` is an object-relational mapper for SQL databases. It is one of the most feature rich and complete ORMs for any language, and is wildly popular and beloved.

```python
from sqlalchemy import String, ForeignKey
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column, relationship

class Base(DeclarativeBase):
    pass


class Employee(Base):
    __tablename__ = "employee"

    wwid: Mapped[int] = mapped_column(primary_key=True)
    first_name: Mapped[str] = mapped_column(String(64))
    last_name: Mapped[str] = mapped_column(String(64))

    team: Mapped["Team"] = relationship(back_populates="employees")


class Team(Base):
    __tablename__ = "team"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(128))

    employees: Mapped[list[Employee]] = relationship(back_populates="team")
```

Once your tables are definined, you can use an engine to load the DB schema and perform queries.

```python
from sqlalchemy import create_engine, select


engine = create_engine("sqlite://")


# create tables
Base.metadata.create_all(engine)


with Session(engine) as session:
    stmt = select(Employee).where(Employee.first_name == "Bob")

    for employee in session.scalars(stmt):
        print(employee.last_name)
```

### `alembic`(https://alembic.sqlalchemy.org/en/latest/)

`alembic` is a migration tool for use with `sqlalchemy`. As you adjust your models over the lifetime of your application alembic will let you automatically create (and adjust if necessary) migration scripts so you can roll forward and backward for database as the schema changes.

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)

## [`litestar`](https://litestar.dev)

`litestar` is a web framework that leverages `pydantic` to allow you to very easily create heavily validate web APIs, complete with powerful, verbose, and automated error messages.

```python
from litestar import Litestar, get, post
from pydantic import BaseModel


class Person(BaseModel):
    name: str
    age: int


people: dict[str, Person] = {}


@get("/person/{name:str}")
async def get_person(name: str) -> Person:
    person = people.get(name)
    return person


@post("/person")
async def create_person(data: Person) -> Person:
    people[person.name] = person
    return person


app = Litestar(route_handlers=[get_person, post_person])
```

[Table of Contents](#table-of-contents)

[Back to Advanced Topics](/advanced/README.md)
