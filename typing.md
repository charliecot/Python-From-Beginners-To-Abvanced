# Python `typing` Module — Complete Beginner-to-Advanced Guide

**Python version:** 3.14  
**Module:** `typing`  
**Level:** Beginner to Advanced  
**Purpose:** Understanding type annotations, static type checking, generic programming, callable types, collection interfaces, and runtime type introspection.

---

# Table of Contents

1. Understanding Python's Type System
2. What Are Type Hints?
3. What Are Type Annotations?
4. Static Type Checking vs Runtime Validation
5. The `typing` Module
6. Basic Type Annotations
7. Any
8. Union
9. Optional
10. Literal
11. Collection Type Annotations
12. TypedDict
13. Required and NotRequired
14. ReadOnly
15. Callable
16. Understanding Iteration
17. Iterable
18. Iterator
19. Sequence
20. Mapping
21. MutableMapping
22. Protocol
23. TypeVar
24. Generic
25. NamedTuple
26. ClassVar
27. Final
28. NoReturn and Never
29. Self
30. TypeGuard and TypeIs
31. Annotated
32. ParamSpec
33. Concatenate
34. Unpack
35. TypeAlias
36. cast
37. reveal_type and assert_type
38. Type Introspection
39. Forward References
40. Modern Python Typing Syntax
41. `typing` vs `collections.abc`
42. TypedDict vs Pydantic BaseModel
43. Practical FastAPI Application
44. Common Mistakes
45. Learning Exercises
46. Quick Reference

---

# PART 1 — UNDERSTANDING PYTHON'S TYPE SYSTEM

## 1. What is a type?

Before understanding the `typing` module, you must understand what a type is.

A type describes the kind of value an object represents.

For example:

```python
age = 25
username = "Charles"
price = 50.5
is_active = True
```

Python assigns each value a type.

```python
print(type(age))
print(type(username))
print(type(price))
print(type(is_active))
```

Output:

```text
<class 'int'>
<class 'str'>
<class 'float'>
<class 'bool'>
```

Let's understand these types:

| Type | Meaning | Example |
|---|---|---|
| `int` | Integer numbers | `25`, `100`, `-5` |
| `str` | Text | `"Charles"` |
| `float` | Decimal numbers | `20.5`, `3.14` |
| `bool` | Boolean values | `True`, `False` |
| `list` | Ordered collection | `[1, 2, 3]` |
| `dict` | Key-value collection | `{"name": "Charles"}` |
| `tuple` | Immutable ordered collection | `(1, 2, 3)` |
| `set` | Collection of unique values | `{1, 2, 3}` |

### Why do types matter?

Different types support different operations.

For example:

```python
age = 25

print(age + 5)
```

This works because integers support addition.

But:

```python
age = "25"

print(age + 5)
```

This produces a TypeError because Python cannot add a string and an integer using ordinary addition.

Understanding types helps us write programs that behave correctly.

---

## 2. What are type hints?

A type hint is information added to your code to communicate the expected type of a value.

Consider:

```python
def add(a, b):
    return a + b
```

Looking at this function, we cannot immediately know:

- Should `a` be an integer?
- Should `b` be an integer?
- Can they be strings?
- What should the function return?

We can communicate our expectations using type hints.

```python
def add(a: int, b: int) -> int:
    return a + b
```

Let's break this down.

```python
a: int
```

Means that `a` is expected to be an integer.

```python
b: int
```

Means that `b` is expected to be an integer.

```python
-> int
```

Means that the function is expected to return an integer.

Example:

```python
result = add(10, 20)

print(result)
```

Output:

```text
30
```

### Important: Do type hints enforce types?

No.

Python does not automatically enforce ordinary type hints.

This is valid Python:

```python
def add(a: int, b: int) -> int:
    return a + b


result = add("Hello", "World")

print(result)
```

Output:

```text
HelloWorld
```

Python executes the function because the `+` operator supports string concatenation.

A static type checker can report that the arguments do not match the declared types.

This distinction is very important throughout this document.

---

## 3. What are annotations?

Python stores annotations as metadata associated with functions, classes, and variables.

Example:

```python
def add(a: int, b: int) -> int:
    return a + b
```

You can inspect the annotations:

```python
print(add.__annotations__)
```

Possible output:

```python
{
    'a': int,
    'b': int,
    'return': int
}
```

You can also annotate variables:

```python
username: str = "Charles"

age: int = 25
```

Annotations communicate the intended types.

They do not automatically validate ordinary variable assignments.

### Annotation vs assignment

Look at:

```python
age: int = 25
```

There are two separate things:

- `age: int` is the annotation.
- `= 25` assigns a value.

You can also annotate without immediately assigning:

```python
age: int
```

This declares the expected type but does not create a value.

Attempting to read `age` before assigning it will cause an error.

---

## 4. Static type checking vs runtime validation

These are two different concepts.

### 4.1 Static type checking

Static type checking means examining your code to detect type inconsistencies without executing the program normally.

Examples of tools:

- Pyright
- mypy
- ty

Consider:

```python
def greet(username: str) -> str:
    return "Hello " + username


greet(100)
```

A static type checker can report:

```text
Expected str
Received int
```

The checker examines the code and reports that the argument does not match the function's annotation.

### 4.2 Runtime validation

Runtime validation means checking actual data while the program is executing.

For example, Pydantic:

```python
from pydantic import BaseModel


class User(BaseModel):
    username: str
    age: int


user = User(
    username="Charles",
    age=25
)
```

Pydantic validates the data when creating the model.

Incorrect input can produce a validation error:

```python
user = User(
    username="Charles",
    age="invalid"
)
```

Pydantic rejects the invalid integer value.

### Comparison

| Static type checking | Runtime validation |
|---|---|
| Examines code and inferred types | Checks actual values |
| Usually happens during development | Happens while the application runs |
| Examples: Pyright, ty, mypy | Examples: Pydantic, manual validation |
| Does not normally execute the application | Executes validation logic |
| Helps detect programming mistakes | Helps validate incoming data |

**Remember:** The `typing` module primarily describes types for static analysis and introspection. Pydantic provides runtime validation when configured to do so.

---

# PART 2 — THE TYPING MODULE

## 5. What is the `typing` module?

The `typing` module is part of Python's standard library.

It provides additional tools for describing types.

Import it:

```python
import typing
```

Or import individual members:

```python
from typing import Any, TypedDict, Literal
```

### Why was it introduced?

Basic annotations are enough for simple functions:

```python
def add(a: int, b: int) -> int:
    return a + b
```

But consider a complicated dictionary:

```python
user = {
    "id": 1,
    "username": "Charles",
    "active": True,
    "permissions": ["read", "write"]
}
```

How do we describe the exact structure of this dictionary?

What if a function accepts any iterable but not necessarily a list?

What if a function accepts another function as an argument?

What if we want to create a reusable class that works with different data types?

These are some of the problems the `typing` module helps solve.

---

# PART 3 — BASIC TYPE CONCEPTS

## 6. Any

### What is Any?

`Any` means that the type checker should treat a value as an unrestricted type.

Import:

```python
from typing import Any
```

Example:

```python
value: Any = 100

value = "Charles"

value = True

value = [1, 2, 3]

value = {"name": "Charles"}
```

The type checker generally permits these different assignments.

### Why do we need Any?

Sometimes you receive data whose structure or type is unknown.

For example, consider a function that accepts arbitrary JSON data.

```python
from typing import Any


def process_data(data: Any) -> Any:
    return data
```

The function can receive different kinds of data.

```python
process_data(100)

process_data("Hello")

process_data({"name": "Charles"})
```

### Any vs object

This distinction is important.

`Any` tells the type checker that it can allow operations without requiring type narrowing.

```python
from typing import Any

value: Any = "Charles"

print(value.upper())
```

An `object` can represent any Python object, but the type checker does not assume it supports string methods.

```python
value: object = "Charles"

# A static type checker will reject this:
# value.upper()
```

You must first establish the type:

```python
if isinstance(value, str):
    print(value.upper())
```

Comparison:

| Any | object |
|---|---|
| Allows almost any operation in static checking | Requires appropriate type narrowing |
| Weakens type safety | Preserves type safety |
| Useful for unknown or dynamic data | Useful for values of genuinely unknown type |

**Recommendation:** Prefer `object` when you want to accept an unknown value but still maintain type safety.

---

## 7. Union

### What is Union?

`Union` means a value can belong to one of several specified types.

Import:

```python
from typing import Union
```

Example:

```python
user_id: Union[int, str]
```

This means `user_id` can be an integer or a string.

```python
user_id = 100

user_id = "USER-001"
```

Modern Python syntax:

```python
user_id: int | str
```

Both forms describe the same type relationship.

### Why do we need Union?

Imagine a function that accepts either an integer ID or a string ID.

```python
def get_user(user_id: int | str):
    print(user_id)
```

Both calls are valid according to the annotation:

```python
get_user(10)

get_user("USER-001")
```

### Multiple possible types

```python
value: int | str | float | bool
```

The value can be one of these types.

### How do we determine the actual type?

Use `isinstance()`.

```python
def process_value(value: int | str):

    if isinstance(value, int):
        print("Integer:", value)

    elif isinstance(value, str):
        print("String:", value)
```

### Important

`Union` does not automatically convert values.

It only describes the acceptable type alternatives.

---

## 8. Optional

### What is Optional?

`Optional` means that a value can be a specified type or `None`.

Import:

```python
from typing import Optional
```

Example:

```python
username: Optional[str] = None
```

This is equivalent to:

```python
username: str | None = None
```

### What is None?

`None` is a special Python object representing the absence of a value.

Example:

```python
result = None

print(result is None)
```

Output:

```text
True
```

### Why do we need Optional?

Imagine a function that searches for a user.

The user might exist or might not exist.

```python
def find_user(user_id: int) -> str | None:

    if user_id == 1:
        return "Charles"

    return None
```

The return annotation communicates two possibilities:

- A string if the user exists.
- `None` if the user does not exist.

Usage:

```python
username = find_user(1)

if username is not None:
    print(username.upper())
else:
    print("User not found")
```

### Important distinction

These two declarations are not identical:

```python
def search(username: str | None):
    ...
```

```python
def search(username: str | None = None):
    ...
```

The first parameter is required, but its value may be `None`.

The second parameter can be omitted because it has a default value.

---

## 9. Literal

### What is Literal?

`Literal` restricts a value to specific permitted values.

Import:

```python
from typing import Literal
```

Example:

```python
status: Literal["pending", "approved", "rejected"]
```

This means the expected value must be one of the listed strings.

Function example:

```python
def update_status(
    status: Literal["pending", "approved", "rejected"]
) -> None:

    print(status)
```

Valid:

```python
update_status("pending")

update_status("approved")
```

Invalid according to static typing:

```python
update_status("completed")
```

### Why use Literal?

Imagine a function that accepts only particular HTTP methods.

```python
HTTPMethod = Literal[
    "GET",
    "POST",
    "PUT",
    "PATCH",
    "DELETE"
]
```

Now:

```python
def send_request(method: HTTPMethod):
    print(method)
```

The type checker can detect unsupported values.

### Literal vs Enum

Both can represent a fixed set of choices.

`Literal`:

```python
from typing import Literal

Genre = Literal[
    "Metal",
    "Alternative",
    "Pop Rock"
]
```

Enum:

```python
from enum import Enum


class Genre(Enum):
    METAL = "Metal"
    ALTERNATIVE = "Alternative"
    POP_ROCK = "Pop Rock"
```

Use `Literal` for simple fixed values in type annotations.

Use Enum when you want named members, reusable values, iteration over choices, and enum-specific behavior.

---

# PART 4 — COLLECTION TYPE ANNOTATIONS

## 10. Understanding collection annotations

A collection stores multiple values.

Examples:

```python
names = ["Charles", "John"]

user = {
    "id": 1,
    "name": "Charles"
}

coordinates = (10, 20)

numbers = {1, 2, 3}
```

We often need to describe not only the collection itself, but also the types of values inside it.

Modern Python supports generic built-in types.

### 10.1 list

Syntax:

```python
list[ITEM_TYPE]
```

Example:

```python
names: list[str] = [
    "Charles",
    "John",
    "Peter"
]
```

Every element is expected to be a string.

Another example:

```python
numbers: list[int] = [10, 20, 30]
```

Mixed types:

```python
values: list[int | str] = [
    10,
    "Charles",
    20
]
```

### 10.2 dict

A dictionary stores key-value pairs.

Syntax:

```python
dict[KEY_TYPE, VALUE_TYPE]
```

Example:

```python
user: dict[str, int] = {
    "age": 25,
    "score": 90
}
```

Here:

- Keys must be strings.
- Values must be integers.

Nested dictionary:

```python
users: dict[str, dict[str, int | str]] = {
    "user1": {
        "id": 1,
        "name": "Charles"
    }
}
```

Understand the nesting:

```python
dict[
    str,
    dict[
        str,
        int | str
    ]
]
```

The outer dictionary has string keys.

Its values are dictionaries.

The inner dictionaries have string keys and values that can be integers or strings.

### 10.3 tuple

A tuple is an ordered collection that cannot be modified after creation.

Example:

```python
user = (1, "Charles")
```

Describe the exact positions:

```python
user: tuple[int, str]
```

This means:

- First element: integer.
- Second element: string.

A tuple containing any number of integers:

```python
numbers: tuple[int, ...] = (1, 2, 3, 4)
```

The ellipsis means any number of additional elements of the specified type.

### 10.4 set

A set stores unique values.

```python
numbers: set[int] = {1, 2, 3}
```

A set does not preserve duplicate values.

```python
numbers = {1, 1, 2, 3}

print(numbers)
```

### 10.5 frozenset

A `frozenset` is an immutable set.

```python
permissions: frozenset[str] = frozenset({
    "read",
    "write"
})
```

You cannot add or remove elements from a frozenset.

### Older collection annotations

Older Python code often uses:

```python
from typing import List, Dict, Tuple, Set
```

Examples:

```python
names: List[str]

user: Dict[str, int]

coordinates: Tuple[int, int]

numbers: Set[int]
```

Modern Python supports:

```python
names: list[str]

user: dict[str, int]

coordinates: tuple[int, int]

numbers: set[int]
```

Prefer the modern built-in syntax.

---

# PART 5 — TYPEDDICT

## 11. Understanding TypedDict

### 11.1 First understand the problem

Imagine you have a dictionary:

```python
user = {
    "id": 1,
    "username": "Charles",
    "email": "charles@example.com"
}
```

Python dictionaries are flexible.

You can write:

```python
user = {
    "id": "hello",
    "username": 100,
    "email": True
}
```

Python allows this because an ordinary dictionary does not define a required structure.

But imagine you are building an application where:

- `id` must be an integer.
- `username` must be a string.
- `email` must be a string.

You need a way to describe this expected structure.

This is where `TypedDict` comes in.

### 11.2 What is TypedDict?

`TypedDict` is a typing construct used to describe the expected structure of a dictionary.

Import:

```python
from typing import TypedDict
```

Define a dictionary structure:

```python
class User(TypedDict):
    id: int
    username: str
    email: str
```

Let's understand each line.

```python
class User(TypedDict):
```

We are defining a TypedDict type named `User`.

```python
id: int
```

The dictionary should contain an `id` key with an integer value.

```python
username: str
```

The dictionary should contain a `username` key with a string value.

```python
email: str
```

The dictionary should contain an `email` key with a string value.

### 11.3 Creating a dictionary

```python
user: User = {
    "id": 1,
    "username": "Charles",
    "email": "charles@example.com"
}
```

The annotation:

```python
user: User
```

tells the static type checker that the dictionary should follow the structure of `User`.

### 11.4 What happens with incorrect data?

```python
user: User = {
    "id": "one",
    "username": "Charles",
    "email": "charles@example.com"
}
```

The type checker should report that `id` has the wrong type.

But ordinary Python execution does not automatically reject this dictionary.

Why?

Because `TypedDict` is primarily a static typing construct.

It does not perform runtime validation.

### 11.5 Nested TypedDict

Now let's connect this to your FastAPI bands application.

Suppose your data looks like this:

```python
BANDS = [
    {
        "id": 1,
        "name": "Coldplay",
        "album": [
            {
                "id": 1,
                "title": "Parachutes",
                "release_year": "2000-07-10"
            }
        ]
    }
]
```

Notice that:

- `BANDS` is a list.
- Every item in the list is a dictionary representing a band.
- Every band contains an `album` list.
- Every album is another dictionary.

We can describe this structure using two TypedDict classes.

```python
from typing import TypedDict


class AlbumData(TypedDict):
    id: int
    title: str
    release_year: str


class BandData(TypedDict):
    id: int
    name: str
    album: list[AlbumData]
```

Now declare your data:

```python
BANDS: list[BandData] = [
    {
        "id": 1,
        "name": "Coldplay",
        "album": [
            {
                "id": 1,
                "title": "Parachutes",
                "release_year": "2000-07-10"
            }
        ]
    }
]
```

### 11.6 Why does this solve the earlier type-checking error?

Previously, you had:

```python
BANDS: list[
    dict[
        str,
        int | str | list[dict[str, int | str]]
    ]
]
```

This says that dictionary values can be integers, strings, or lists of dictionaries.

When you access:

```python
b["album"]
```

the type checker knows only that the value could be one of those alternatives.

It cannot guarantee that the result is always a list.

With `TypedDict`:

```python
class BandData(TypedDict):
    id: int
    name: str
    album: list[AlbumData]
```

The type checker knows exactly what the `album` key contains.

Therefore:

```python
b["album"]
```

has the type:

```python
list[AlbumData]
```

That is much more precise.

### 11.7 TypedDict properties

You can inspect a TypedDict:

```python
print(User.__annotations__)

print(User.__total__)

print(User.__required_keys__)

print(User.__optional_keys__)
```

These expose information about the TypedDict definition.

Important: `TypedDict` is not a normal dictionary subclass that validates its contents.

It is a special typing construct used by static type checkers and introspection tools.

---

# PART 6 — REQUIRED, NOTREQUIRED AND READONLY

## 12. Required and NotRequired

By default, all TypedDict keys are required.

Consider:

```python
from typing import TypedDict


class User(TypedDict):
    id: int
    username: str
    email: str
```

A user dictionary is expected to contain all three keys.

But what if `email` is optional?

Use `NotRequired`.

```python
from typing import TypedDict, NotRequired


class User(TypedDict):
    id: int
    username: str
    email: NotRequired[str]
```

Now these are both valid:

```python
user1: User = {
    "id": 1,
    "username": "Charles"
}
```

```python
user2: User = {
    "id": 2,
    "username": "John",
    "email": "john@example.com"
}
```

### Required

`Required` explicitly marks a key as required.

```python
from typing import TypedDict, Required, NotRequired


class User(TypedDict, total=False):
    id: Required[int]
    username: Required[str]
    email: NotRequired[str]
```

Here:

- `id` is required.
- `username` is required.
- `email` is optional.

### What does total=False mean?

Normally, TypedDict treats all keys as required.

`total=False` changes the default so that keys are optional.

You can selectively make some keys required using `Required`.

---

## 13. ReadOnly

`ReadOnly` describes a TypedDict item that should not be reassigned through that TypedDict interface.

Available in Python 3.13+.

```python
from typing import TypedDict, ReadOnly


class User(TypedDict):
    id: ReadOnly[int]
    username: str
```

The `id` field is marked read-only for static type checking.

This is useful when a field should not be changed through a particular interface.

Important:

`ReadOnly` does not make the underlying dictionary physically immutable at runtime.

It is a typing restriction.

---

# PART 7 — CALLABLE

## 14. Understanding Callable

Before understanding `Callable`, you need to understand what a callable is.

A callable is an object that can be called using parentheses.

For example:

```python
def greet():
    print("Hello")
```

You call the function using:

```python
greet()
```

Functions are callable objects.

Classes are also callable:

```python
class User:
    pass


user = User()
```

The class `User` is called to create an instance.

Other objects can also be callable if they implement `__call__()`.

### 14.1 Why do we need Callable?

Imagine you want to write a function that receives another function as an argument.

```python
def execute(operation):
    return operation(10, 20)
```

What type of object is `operation`?

It is expected to be a callable that accepts two arguments and returns a result.

We can describe this with `Callable`.

Import:

```python
from typing import Callable
```

### 14.2 Callable syntax

```python
Callable[[PARAMETER_TYPES], RETURN_TYPE]
```

For example:

```python
Callable[[int, int], int]
```

Means:

- Accept two integer parameters.
- Return an integer.

Complete example:

```python
from typing import Callable


def execute(
    operation: Callable[[int, int], int]
) -> int:

    return operation(10, 20)


def add(a: int, b: int) -> int:
    return a + b


print(execute(add))
```

Output:

```text
30
```

### 14.3 Understanding the execution

First:

```python
def add(a: int, b: int) -> int:
    return a + b
```

We define a function.

Next:

```python
execute(add)
```

We pass the function itself.

Notice that we write:

```python
add
```

not:

```python
add()
```

The first passes the function object.

The second executes the function immediately.

Inside `execute`:

```python
operation(10, 20)
```

Python calls the function that was supplied.

Since `operation` refers to `add`, this becomes:

```python
add(10, 20)
```

### 14.4 Callable with no arguments

```python
Callable[[], str]
```

Means:

- Takes no arguments.
- Returns a string.

Example:

```python
def get_name() -> str:
    return "Charles"


def execute(func: Callable[[], str]) -> str:
    return func()


print(execute(get_name))
```

### 14.5 Callable with arbitrary arguments

```python
Callable[..., str]
```

The ellipsis indicates that the callable's parameter types are not specified.

The expected return type is still a string.

Use this only when you genuinely do not need to describe the parameter signature precisely.

---

# PART 8 — UNDERSTANDING ITERATION

Before learning `Iterable` and `Iterator`, you need to understand how Python iteration works.

## 15. What is iteration?

Iteration means going through a collection or sequence of values, one at a time.

Example:

```python
names = ["Charles", "John", "Peter"]

for name in names:
    print(name)
```

Output:

```text
Charles
John
Peter
```

The `for` loop retrieves each item one at a time.

But how does Python know how to retrieve those items?

Python uses the iteration protocol.

---

## 16. The iteration protocol

Two important methods are involved:

```python
__iter__()
```

and:

```python
__next__()
```

### 16.1 What is __iter__()?

`__iter__()` returns an iterator.

Example:

```python
names = ["Charles", "John", "Peter"]

iterator = iter(names)
```

The built-in `iter()` function obtains an iterator from the list.

Conceptually, it calls:

```python
names.__iter__()
```

### 16.2 What is __next__()?

`__next__()` retrieves the next available item.

```python
names = ["Charles", "John", "Peter"]

iterator = iter(names)

print(next(iterator))
print(next(iterator))
print(next(iterator))
```

Output:

```text
Charles
John
Peter
```

The built-in `next()` function calls the iterator's `__next__()` method.

### 16.3 What happens when there are no more values?

```python
names = ["Charles"]

iterator = iter(names)

print(next(iterator))
print(next(iterator))
```

The second call raises:

```text
StopIteration
```

This exception tells Python that iteration has finished.

A `for` loop handles this automatically.

---

## 17. Iterable

### What is an Iterable?

An iterable is an object from which an iterator can be obtained.

Examples:

```python
names = ["Charles", "John"]

coordinates = (10, 20)

permissions = {"read", "write"}

username = "Charles"
```

All of these are iterable.

You can use them in a `for` loop.

### 17.1 Why do we need Iterable?

Imagine writing a function:

```python
def display_names(names):
    for name in names:
        print(name)
```

The function does not really need a list.

It only needs something it can iterate over.

It could receive:

- A list.
- A tuple.
- A set.
- A string.
- A generator.
- Another iterable object.

We can express that requirement using `Iterable`.

Import:

```python
from collections.abc import Iterable
```

Example:

```python
def display_names(names: Iterable[str]) -> None:

    for name in names:
        print(name)
```

Now all of these are acceptable according to the annotation:

```python
display_names(["Charles", "John"])

display_names(("Charles", "John"))

display_names({"Charles", "John"})
```

### 17.2 Why not use list[str]?

Compare:

```python
def display_names(names: list[str]):
    ...
```

This says the function expects a list.

But:

```python
def display_names(names: Iterable[str]):
    ...
```

This says the function accepts any iterable that produces strings.

The second is more flexible.

### 17.3 Important

An iterable does not necessarily support:

```python
items[0]
```

It also does not necessarily support:

```python
len(items)
```

An iterable only promises iteration behavior.

---

## 18. Iterator

### What is an Iterator?

An iterator is an object that produces values one at a time.

An iterator:

- Provides `__iter__()`.
- Provides `__next__()`.
- Raises `StopIteration` when exhausted.

Import:

```python
from collections.abc import Iterator
```

Example:

```python
names = ["Charles", "John", "Peter"]

iterator = iter(names)
```

Retrieve values:

```python
print(next(iterator))
print(next(iterator))
print(next(iterator))
```

### 18.1 Creating your own iterator

```python
class Count:

    def __init__(self, maximum: int):
        self.maximum = maximum
        self.current = 0

    def __iter__(self):
        return self

    def __next__(self):

        if self.current >= self.maximum:
            raise StopIteration

        self.current += 1

        return self.current
```

Usage:

```python
counter = Count(3)

for number in counter:
    print(number)
```

Output:

```text
1
2
3
```

Let's understand:

```python
def __iter__(self):
    return self
```

This object is its own iterator.

```python
def __next__(self):
```

This method returns the next value.

When no values remain:

```python
raise StopIteration
```

Python stops the loop.

### 18.2 Iterator vs Iterable

| Iterable | Iterator |
|---|---|
| Can provide an iterator | Produces values |
| Must provide `__iter__()` | Provides `__iter__()` and `__next__()` |
| Can be a list or tuple | Can be a list iterator or generator |
| May be reusable | Often consumed as values are retrieved |

A list is iterable, but it is not itself an iterator.

```python
names = ["Charles", "John"]

print(iter(names))
```

The result is a list iterator.

### 18.3 Generator functions

Generators are a convenient way to create iterators.

A function becomes a generator function when it uses `yield`.

```python
def generate_numbers():
    yield 1
    yield 2
    yield 3
```

Use it:

```python
numbers = generate_numbers()

print(next(numbers))
print(next(numbers))
print(next(numbers))
```

Annotate it:

```python
from collections.abc import Iterator


def generate_numbers() -> Iterator[int]:
    yield 1
    yield 2
    yield 3
```

---

## 19. Sequence

### What is a Sequence?

A sequence is an ordered collection that supports operations such as:

- Accessing elements by index.
- Getting its length.
- Iterating over elements.

Examples:

```python
names = ["Charles", "John"]

coordinates = (10, 20)

username = "Charles"
```

These are common sequence types.

Import:

```python
from collections.abc import Sequence
```

Example:

```python
def display_first(items: Sequence[str]) -> None:

    if len(items) > 0:
        print(items[0])
```

Usage:

```python
display_first(["Charles", "John"])

display_first(("Charles", "John"))
```

### Why use Sequence instead of list?

Because a function may only need an ordered collection with indexing.

It does not necessarily need to modify the collection.

Using:

```python
Sequence[str]
```

allows lists, tuples, and other compatible sequence objects.

Using:

```python
list[str]
```

requires a list specifically.

### Sequence vs Iterable

An iterable only guarantees iteration.

A sequence provides additional behavior such as indexing and length.

---

## 20. Mapping

### What is Mapping?

A mapping is an object that associates keys with values.

A dictionary is the most common mapping.

Example:

```python
user = {
    "id": 1,
    "username": "Charles"
}
```

You retrieve values using keys:

```python
print(user["username"])
```

Import:

```python
from collections.abc import Mapping
```

Example:

```python
def display_user(user: Mapping[str, int | str]) -> None:

    print(user["username"])
    print(user["id"])
```

The annotation means:

- Keys are strings.
- Values are integers or strings.
- The function expects a mapping interface.

### Why use Mapping instead of dict?

A function might only need to read key-value pairs.

It does not necessarily need to modify the dictionary.

`Mapping` describes that read-oriented interface.

Other objects can implement the mapping interface too.

---

## 21. MutableMapping

A mutable mapping is a mapping that supports modification.

Import:

```python
from collections.abc import MutableMapping
```

Example:

```python
def add_user(
    users: MutableMapping[str, int],
    username: str,
    age: int
) -> None:

    users[username] = age
```

Usage:

```python
users = {
    "Charles": 25
}

add_user(users, "John", 30)

print(users)
```

Output:

```python
{
    "Charles": 25,
    "John": 30
}
```

Difference:

| Mapping | MutableMapping |
|---|---|
| Read-oriented interface | Supports reading and modifying |
| Provides mapping operations | Adds modification operations |
| Does not promise assignment support | Supports assignment and deletion |

---

# PART 9 — PROTOCOL

## 22. Understanding Protocol

Before understanding `Protocol`, consider two classes:

```python
class Dog:

    def speak(self):
        return "Woof"


class Cat:

    def speak(self):
        return "Meow"
```

Notice something:

Neither class inherits from the other.

But both have a method called:

```python
speak()
```

Now create a function:

```python
def make_sound(animal):
    return animal.speak()
```

Both objects can be passed to the function.

```python
print(make_sound(Dog()))

print(make_sound(Cat()))
```

This works because both objects provide the required method.

### 22.1 What problem does Protocol solve?

Suppose you want to communicate that a function accepts any object that provides a `speak()` method returning a string.

You do not necessarily want every class to inherit from the same base class.

This is where `Protocol` comes in.

Import:

```python
from typing import Protocol
```

Define the protocol:

```python
class Animal(Protocol):

    def speak(self) -> str:
        ...
```

Let's understand the method:

```python
def speak(self) -> str:
```

An object must provide a `speak()` method that returns a string.

```python
...
```

The ellipsis indicates that this is a declaration without an implementation.

### 22.2 Use the Protocol

```python
class Dog:

    def speak(self) -> str:
        return "Woof"


class Cat:

    def speak(self) -> str:
        return "Meow"
```

Function:

```python
def make_sound(animal: Animal) -> str:
    return animal.speak()
```

Usage:

```python
print(make_sound(Dog()))

print(make_sound(Cat()))
```

The type checker recognizes that both classes satisfy the protocol because they implement the required method.

Neither class needs to explicitly inherit from `Animal`.

### 22.3 Structural typing vs nominal typing

These are two approaches to type compatibility.

**Nominal typing**

A class is considered compatible based on its declared inheritance relationship.

```python
class Animal:
    pass


class Dog(Animal):
    pass
```

Dog inherits from Animal.

**Structural typing**

An object is considered compatible based on whether it provides the required members.

```python
class Dog:

    def speak(self) -> str:
        return "Woof"
```

Dog can satisfy the Animal protocol without inheriting from it.

### 22.4 runtime_checkable

Normally, Protocol is primarily used by static type checkers.

If you want limited runtime structural checks, use `runtime_checkable`.

```python
from typing import Protocol, runtime_checkable


@runtime_checkable
class Animal(Protocol):

    def speak(self) -> str:
        ...


class Dog:

    def speak(self) -> str:
        return "Woof"


dog = Dog()

print(isinstance(dog, Animal))
```

Output:

```text
True
```

Important:

Runtime protocol checks primarily verify the presence of required members. They do not perform complete static signature checking.

---

# PART 10 — TYPE VARIABLES AND GENERICS

## 23. TypeVar

### What is TypeVar?

`TypeVar` creates a reusable type variable.

Import:

```python
from typing import TypeVar
```

Before understanding it, consider this function:

```python
def get_first(items):
    return items[0]
```

It retrieves the first item from a list.

But what is the return type?

It depends on the input.

```python
get_first([1, 2, 3])
```

Returns an integer.

```python
get_first(["Charles", "John"])
```

Returns a string.

We want the type checker to understand that the return type depends on the input type.

### 23.1 Create a TypeVar

```python
from typing import TypeVar

T = TypeVar("T")
```

`T` is a type variable.

It is not a normal Python variable containing an integer or string.

It represents a type that will be determined when the generic function is used.

Example:

```python
def get_first(items: list[T]) -> T:
    return items[0]
```

Understand this:

```python
list[T]
```

The input is a list whose elements have type `T`.

```python
-> T
```

The function returns an element of that same type.

Usage:

```python
number = get_first([10, 20, 30])

name = get_first(["Charles", "John"])
```

The type checker can infer:

- `number` is an integer.
- `name` is a string.

### 23.2 Why not use Any?

Consider:

```python
from typing import Any


def get_first(items: list[Any]) -> Any:
    return items[0]
```

This loses the relationship between the input element type and the return type.

`TypeVar` preserves that relationship.

### 23.3 Bounded TypeVar

A bounded TypeVar restricts the allowed type to a particular base type or its subclasses.

```python
from typing import TypeVar

T = TypeVar("T", bound=int)
```

This means that `T` must be a subtype of `int`.

Example:

```python
def double(value: T) -> T:
    return value * 2
```

A static type checker can enforce the declared bound.

### 23.4 Constrained TypeVar

A constrained TypeVar accepts only specified types.

```python
from typing import TypeVar

T = TypeVar("T", int, str)
```

The type variable is constrained to the listed alternatives.

This is different from an unrestricted TypeVar.

---

## 24. Generic

### What is Generic?

`Generic` allows you to create classes that work with different data types while preserving type information.

Imagine creating a container.

```python
class Storage:

    def __init__(self, value):
        self.value = value

    def get(self):
        return self.value
```

This container can hold anything.

But the type checker does not have a precise relationship between the type of the stored value and the type returned by `get()`.

We can use generics.

### 24.1 Create a generic class

```python
from typing import Generic, TypeVar

T = TypeVar("T")


class Storage(Generic[T]):

    def __init__(self, value: T):
        self.value = value

    def get(self) -> T:
        return self.value
```

Understand:

```python
class Storage(Generic[T]):
```

This declares a generic class parameterized by `T`.

```python
value: T
```

The constructor accepts a value of type `T`.

```python
def get(self) -> T:
```

The method returns that same type.

### 24.2 Create instances

```python
integer_storage = Storage[int](100)

string_storage = Storage[str]("Python")
```

The type checker understands:

```python
integer_storage.get()
```

returns an integer.

And:

```python
string_storage.get()
```

returns a string.

### 24.3 TypeVar vs Generic

| TypeVar | Generic |
|---|---|
| Defines a type variable | Uses type variables in generic classes |
| Useful for generic functions | Useful for generic classes |
| Represents a reusable type parameter | Makes a class parameterized by types |

---

# PART 11 — NAMEDTUPLE AND CLASS VARIABLES

## 25. NamedTuple

A `NamedTuple` is a tuple with named fields.

Normally:

```python
user = (1, "Charles")
```

You access the values using indexes:

```python
print(user[0])

print(user[1])
```

But remembering what each position represents can become difficult.

`NamedTuple` gives the elements names.

Import:

```python
from typing import NamedTuple
```

Example:

```python
class User(NamedTuple):
    id: int
    username: str
```

Create an instance:

```python
user = User(1, "Charles")
```

Access values:

```python
print(user.id)

print(user.username)

print(user[0])
```

A NamedTuple:

- Is tuple-like.
- Supports indexing.
- Supports unpacking.
- Is immutable.
- Provides named fields.

Use it when you need a lightweight immutable record.

---

## 26. ClassVar

### What is ClassVar?

`ClassVar` indicates that an attribute belongs to the class rather than being an ordinary instance attribute.

First understand the difference.

```python
class User:

    user_count = 0

    def __init__(self, username):
        self.username = username
```

Here:

```python
user_count
```

is a class-level attribute.

Every instance can access it through the class.

```python
User.user_count
```

Now use `ClassVar`:

```python
from typing import ClassVar


class User:

    user_count: ClassVar[int] = 0

    def __init__(self, username: str):
        self.username = username

        User.user_count += 1
```

`ClassVar[int]` tells the type checker that `user_count` is intended to be a class variable.

It does not automatically enforce runtime immutability or prevent instance assignment.

---

## 27. Final

`Final` indicates that a variable or attribute should not be reassigned according to static typing rules.

Example:

```python
from typing import Final

MAX_USERS: Final[int] = 100
```

The intention is that `MAX_USERS` should not be reassigned.

Class example:

```python
from typing import Final


class Configuration:

    APP_NAME: Final[str] = "My API"
```

A type checker can report attempts to reassign the final attribute.

Important:

`Final` does not make an object deeply immutable.

---

# PART 12 — FUNCTIONS THAT DO NOT RETURN

## 28. NoReturn

Sometimes a function never returns normally.

For example:

```python
def terminate():
    raise SystemExit("Application stopped")
```

This function raises an exception instead of returning a normal value.

We can describe that using `NoReturn`.

```python
from typing import NoReturn


def terminate() -> NoReturn:
    raise SystemExit("Application stopped")
```

This tells the type checker that the function does not return normally.

## 29. Never

`Never` is a bottom type: a type with no possible values.

Example:

```python
from typing import Never


def impossible() -> Never:
    raise RuntimeError("This function never returns")
```

### NoReturn vs Never

They are closely related.

- `NoReturn` is commonly used to annotate functions that never return normally.
- `Never` represents the bottom type more generally and can also be used for exhaustive type checking.

---

# PART 13 — SELF AND TYPE NARROWING

## 30. Self

`Self` represents the current class type.

Import:

```python
from typing import Self
```

Example:

```python
class User:

    def set_username(self, username: str) -> Self:
        self.username = username

        return self
```

Usage:

```python
user = User().set_username("Charles")
```

### Why do we need Self?

Consider method chaining:

```python
user.set_username("Charles").set_username("John")
```

`Self` helps the type checker understand that the method returns the current class type.

This is especially useful with inheritance and fluent APIs.

---

## 31. TypeGuard

`TypeGuard` helps a type checker understand that a function checks whether a value belongs to a more specific type.

Import:

```python
from typing import TypeGuard
```

Example:

```python
def is_string(value: object) -> TypeGuard[str]:
    return isinstance(value, str)
```

Now:

```python
value: object = "Charles"

if is_string(value):
    print(value.upper())
```

The type checker understands that `value` is a string inside the successful branch.

### Why do we need TypeGuard?

Sometimes Python cannot determine a type from ordinary conditions.

You can write a custom checking function and communicate its result to the type checker.

### Example with lists

```python
from typing import TypeGuard


def is_string_list(
    values: list[object]
) -> TypeGuard[list[str]]:

    return all(
        isinstance(value, str)
        for value in values
    )
```

Usage:

```python
values: list[object] = [
    "Python",
    "FastAPI"
]

if is_string_list(values):

    for value in values:
        print(value.upper())
```

Important:

`TypeGuard` does not automatically validate values. Your function must perform the actual check.

---

## 32. TypeIs

`TypeIs` is another type-narrowing construct, available in Python 3.13+.

Import:

```python
from typing import TypeIs
```

Example:

```python
def is_integer(value: object) -> TypeIs[int]:
    return isinstance(value, int)
```

Usage:

```python
value: object = 100

if is_integer(value):
    print(value + 10)
```

### TypeGuard vs TypeIs

| TypeGuard | TypeIs |
|---|---|
| Allows narrowing to a type that is not necessarily a subtype of the input type | Requires a subtype relationship |
| Narrows the positive branch | Can narrow both positive and negative branches |
| Useful for custom narrowing | Useful for subtype-based narrowing |

---

# PART 14 — ANNOTATED

## 33. Annotated

`Annotated` allows you to attach additional metadata to a type.

Import:

```python
from typing import Annotated
```

Example:

```python
username: Annotated[str, "Must be a valid username"]
```

Let's understand:

```python
str
```

is the actual type.

```python
"Must be a valid username"
```

is additional metadata.

`Annotated` does not automatically validate the value.

A framework can inspect the metadata and use it.

### 33.1 Annotated with FastAPI

```python
from typing import Annotated

from fastapi import Query


async def search_users(
    username: Annotated[
        str,
        Query(min_length=3, max_length=30)
    ]
):
    return {"username": username}
```

FastAPI interprets the `Query` metadata.

It can enforce the minimum and maximum lengths at runtime.

### 33.2 Annotated with Pydantic

```python
from typing import Annotated

from pydantic import BaseModel, Field


Age = Annotated[
    int,
    Field(ge=0, le=120)
]


class User(BaseModel):
    age: Age
```

Pydantic uses the metadata to validate the age.

This is an important connection between Python typing and Pydantic.

---

# PART 15 — ADVANCED CALLABLE TYPING

## 34. ParamSpec

`ParamSpec` is used to capture the parameter types of a callable.

Before understanding it, consider a decorator.

A decorator wraps another function.

```python
def decorator(func):

    def wrapper(*args, **kwargs):
        print("Executing function")

        return func(*args, **kwargs)

    return wrapper
```

The wrapper accepts arbitrary arguments.

But we want to preserve the original function's parameter types.

This is where `ParamSpec` helps.

Import:

```python
from typing import ParamSpec, TypeVar, Callable

P = ParamSpec("P")

R = TypeVar("R")
```

Example:

```python
def decorator(
    func: Callable[P, R]
) -> Callable[P, R]:

    def wrapper(
        *args: P.args,
        **kwargs: P.kwargs
    ) -> R:

        print("Executing function")

        return func(*args, **kwargs)

    return wrapper
```

Understand:

- `P` represents the parameter specification.
- `P.args` represents positional arguments.
- `P.kwargs` represents keyword arguments.
- `R` represents the return type.

This allows the wrapper to preserve the original function's typing relationship.

---

## 35. Concatenate

`Concatenate` is used with callable typing when you want to add a specific parameter before an existing parameter specification.

Example:

```python
from typing import Callable, Concatenate, ParamSpec, TypeVar

P = ParamSpec("P")

R = TypeVar("R")


def with_user(
    func: Callable[Concatenate[str, P], R]
) -> Callable[P, R]:

    def wrapper(
        *args: P.args,
        **kwargs: P.kwargs
    ) -> R:

        return func("Charles", *args, **kwargs)

    return wrapper
```

The important idea is that the wrapped function receives an additional string argument before the parameters represented by `P`.

This is useful in advanced decorators and dependency-injection systems.

---

## 36. Unpack

`Unpack` expands a typed structure into another typing construct.

A common use is describing keyword arguments using a TypedDict.

```python
from typing import TypedDict, Unpack


class User(TypedDict):
    username: str
    age: int


def create_user(**kwargs: Unpack[User]) -> None:
    print(kwargs)
```

Usage:

```python
create_user(
    username="Charles",
    age=25
)
```

The type checker understands the expected keyword arguments.

---

# PART 16 — TYPE ALIASES

## 37. TypeAlias

A type alias gives a name to an existing type expression.

Imagine you repeatedly use:

```python
int | str | None
```

You can give it a descriptive name.

```python
from typing import TypeAlias

UserID: TypeAlias = int | str
```

Now:

```python
user_id: UserID = 100
```

The alias makes complicated type annotations easier to read.

### Python 3.12+ type statement

Modern Python supports:

```python
type UserID = int | str
```

This creates a type alias object.

### Why use aliases?

Consider:

```python
def process(
    data: dict[
        str,
        list[
            dict[str, int | str]
        ]
    ]
):
    ...
```

This is difficult to read.

You can simplify it:

```python
type AlbumData = dict[str, int | str]

type BandData = dict[str, list[AlbumData]]
```

Then:

```python
def process(data: BandData):
    ...
```

Type aliases improve readability and reuse.

---

# PART 17 — TYPE CASTING AND INSPECTION

## 38. cast

`cast` tells a static type checker to treat an expression as a specified type.

Import:

```python
from typing import cast
```

Example:

```python
value: object = "Python"

text = cast(str, value)

print(text.upper())
```

### What does cast actually do?

It does not convert the value.

It does not validate the value.

It only communicates a type expectation to the static type checker.

This is very important.

Consider:

```python
value: object = 100

text = cast(str, value)

print(text.upper())
```

The cast does not convert `100` into a string.

At runtime, the code will fail because integers do not have the string method `upper()`.

Use `cast` only when you have a valid reason to know the type.

---

## 39. reveal_type

`reveal_type` helps reveal the type inferred by a type checker.

```python
from typing import reveal_type

username = "Charles"

reveal_type(username)
```

The type checker should identify the expression as `str`.

This is useful when dealing with complicated type inference.

---

## 40. assert_type

`assert_type` checks whether the type checker infers the expected type.

```python
from typing import assert_type

username = "Charles"

assert_type(username, str)
```

This is useful when writing tests for complex typing behavior.

It does not perform runtime data validation.

---

# PART 18 — TYPE INTROSPECTION

## 41. What is introspection?

Introspection means examining objects and their properties while a program is running.

Python allows you to inspect:

- Functions.
- Classes.
- Variables.
- Annotations.
- Object types.
- Attributes.

The `typing` module provides helpers for inspecting type expressions.

---

## 42. get_type_hints

`get_type_hints()` returns the resolved annotations of a function or class.

Import:

```python
from typing import get_type_hints
```

Example:

```python
def add(a: int, b: int) -> int:
    return a + b
```

Inspect:

```python
print(get_type_hints(add))
```

Output:

```python
{
    "a": int,
    "b": int,
    "return": int
}
```

Why is this useful?

Frameworks and libraries can inspect annotations to understand how functions should be used.

FastAPI uses type annotations as part of its request and response handling.

---

## 43. get_origin

`get_origin()` retrieves the underlying type constructor of a generic type expression.

Import:

```python
from typing import get_origin
```

Example:

```python
print(get_origin(list[int]))
```

Output:

```text
<class 'list'>
```

Another example:

```python
print(get_origin(dict[str, int]))
```

Output:

```text
<class 'dict'>
```

The origin is the outer type constructor.

---

## 44. get_args

`get_args()` retrieves the arguments contained inside a typing expression.

Import:

```python
from typing import get_args
```

Example:

```python
print(get_args(list[int]))
```

Output:

```python
(int,)
```

Another example:

```python
print(get_args(dict[str, int]))
```

Output:

```python
(str, int)
```

For a union:

```python
print(get_args(int | str))
```

Output:

```python
(int, str)
```

### get_origin vs get_args

Consider:

```python
dict[str, int]
```

- `get_origin()` returns `dict`.
- `get_args()` returns `(str, int)`.

---

# PART 19 — FORWARD REFERENCES

## 45. What is a forward reference?

A forward reference occurs when a type annotation refers to a class that has not yet been defined.

Example:

```python
class Employee:

    def get_manager(self) -> "Manager":
        ...


class Manager:
    pass
```

At the time Python processes the method annotation, `Manager` has not yet been defined.

The string `"Manager"` allows the reference to be resolved later.

### Future annotations

You can also use:

```python
from __future__ import annotations
```

This postpones annotation evaluation.

Example:

```python
from __future__ import annotations


class Employee:

    def get_manager(self) -> Manager:
        ...


class Manager:
    pass
```

This is particularly useful when classes refer to each other.

---

# PART 20 — MODERN PYTHON TYPING

## 46. Older syntax vs modern syntax

Python has introduced more convenient syntax for type annotations.

| Older syntax | Modern syntax |
|---|---|
| `List[str]` | `list[str]` |
| `Dict[str, int]` | `dict[str, int]` |
| `Tuple[int, str]` | `tuple[int, str]` |
| `Set[int]` | `set[int]` |
| `FrozenSet[str]` | `frozenset[str]` |
| `Union[int, str]` | `int \| str` |
| `Optional[str]` | `str \| None` |
| `Type[User]` | `type[User]` |
| `TypeAlias` | `type` statement for aliases |

For Python 3.14, prefer the modern syntax where possible.

Example:

```python
def get_user(
    user_id: int | str
) -> dict[str, int | str] | None:
    ...
```

---

# PART 21 — TYPING VS COLLECTIONS.ABC

## 47. What is collections.abc?

Python has another module:

```python
collections.abc
```

It provides abstract collection interfaces.

These interfaces describe behaviors that objects can support.

Examples:

```python
Iterable
Iterator
Sequence
Mapping
MutableMapping
Callable
```

Import:

```python
from collections.abc import (
    Iterable,
    Iterator,
    Sequence,
    Mapping,
    MutableMapping,
    Callable,
)
```

### Why does this matter?

The `typing` module historically provided aliases for many of these concepts.

Modern Python supports using the corresponding abstract base classes from `collections.abc` directly for annotations.

### Concrete types vs abstract interfaces

Consider:

```python
def process(items: list[str]):
    ...
```

This expects a list specifically.

Now:

```python
def process(items: Iterable[str]):
    ...
```

This accepts any compatible iterable of strings.

The second function is more flexible.

### When should you use each?

Use concrete types when you require a particular collection.

```python
def modify_users(users: list[str]):
    users.append("Charles")
```

Use abstract interfaces when you only need a behavior.

```python
def display_users(users: Iterable[str]):
    for user in users:
        print(user)
```

---

# PART 22 — TYPEDDICT VS PYDANTIC

## 48. Understanding the difference

This distinction is particularly important for FastAPI development.

### TypedDict

```python
from typing import TypedDict


class User(TypedDict):
    id: int
    username: str
```

Usage:

```python
user: User = {
    "id": 1,
    "username": "Charles"
}
```

TypedDict describes the expected dictionary structure for static type checking.

It does not automatically validate the dictionary at runtime.

### Pydantic BaseModel

```python
from pydantic import BaseModel


class User(BaseModel):
    id: int
    username: str
```

Usage:

```python
user = User(
    id=1,
    username="Charles"
)
```

Pydantic validates and parses the supplied data at runtime.

### Comparison

| Feature | TypedDict | Pydantic BaseModel |
|---|---|---|
| Describes dictionary structure | Yes | Yes |
| Static type checking | Yes | Yes |
| Runtime validation | No | Yes |
| Converts input values | No | According to validation rules |
| Creates model instances | No, it describes dictionaries | Yes |
| Nested validation | Static typing | Runtime validation |
| Useful for raw dictionary data | Yes | Also possible |

### Which should you use?

Use `TypedDict` when:

- You are describing an existing dictionary.
- You want precise static typing for dictionary data.
- You do not need runtime validation from that type.

Use Pydantic when:

- You need to validate external data.
- You are building API request and response schemas.
- You need nested runtime validation.
- You want serialization and validation features.

You can use both in the same application.

---

# PART 23 — PRACTICAL FASTAPI EXAMPLE

## 49. Putting the concepts together

Let's use your bands application.

First, define the source dictionary structures.

```python
from typing import TypedDict
from enum import Enum
from datetime import date


class GenreURLChoices(str, Enum):
    ALTERNATIVE = "Alternative"
    POP_ROCK = "Pop Rock"
    METAL = "Metal"
    HEAVY = "Heavy"


class AlbumData(TypedDict):
    id: int
    title: str
    release_year: str


class BandData(TypedDict):
    id: int
    name: str
    genre: GenreURLChoices
    album: list[AlbumData]
```

Now define the Pydantic models.

```python
from pydantic import BaseModel, Field


class Albums(BaseModel):
    id: int
    title: str
    release_year: date


class Band(BaseModel):
    id: int
    name: str
    genre: GenreURLChoices
    album: list[Albums] = Field(default_factory=list)
```

Now define your data.

```python
BANDS: list[BandData] = [
    {
        "id": 1,
        "name": "Coldplay",
        "genre": GenreURLChoices.ALTERNATIVE,
        "album": [
            {
                "id": 1,
                "title": "Parachutes",
                "release_year": "2000-07-10"
            }
        ]
    }
]
```

Now use FastAPI.

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/bands", response_model=list[Band])
async def get_bands() -> list[Band]:

    return [
        Band(**band)
        for band in BANDS
    ]
```

Let's identify where each typing concept is used.

| Code | Concept |
|---|---|
| `list[BandData]` | Collection type annotation |
| `BandData` | TypedDict |
| `AlbumData` | Nested TypedDict |
| `list[AlbumData]` | List of nested dictionaries |
| `list[Albums]` | List of Pydantic models |
| `date` | Date type |
| `GenreURLChoices` | Enum |
| `Field(default_factory=list)` | Pydantic default factory |
| `response_model=list[Band]` | FastAPI response schema |

Notice the difference:

```python
BANDS: list[BandData]
```

describes your original data.

```python
response_model=list[Band]
```

describes the response structure that FastAPI should use.

---

# PART 24 — COMMON MISTAKES

## 50. Mistake: Assuming type hints enforce values

Incorrect assumption:

```python
age: int = "Charles"
```

Python does not automatically reject this.

Use a static type checker or runtime validation when needed.

## 51. Mistake: Using Any everywhere

```python
def process(data: Any):
    ...
```

This removes much of the benefit of static checking.

Prefer a more precise type.

## 52. Mistake: Confusing Optional with an optional argument

```python
def get_user(user_id: int | None):
    ...
```

The argument is still required.

To make it omittable:

```python
def get_user(user_id: int | None = None):
    ...
```

## 53. Mistake: Thinking Iterable means list

A list is iterable, but an iterable does not have to be a list.

## 54. Mistake: Thinking TypedDict validates data

TypedDict does not automatically validate dictionaries at runtime.

Use Pydantic for runtime validation.

## 55. Mistake: Thinking cast converts values

```python
cast(str, 100)
```

Does not convert `100` into `"100"`.

It only informs the type checker.

## 56. Mistake: Confusing Callable with calling a function

```python
execute(add)
```

Passes the function object.

```python
execute(add())
```

Calls the function immediately and passes its return value.

These are different operations.

## 57. Mistake: Using list when you only need Iterable

If your function only loops over values, consider:

```python
Iterable[str]
```

instead of:

```python
list[str]
```

This allows more kinds of input.

---

# PART 25 — PRACTICAL EXERCISES

## 58. Exercise 1: Basic annotations

Write a function that:

- Accepts two integers.
- Returns an integer.
- Calculates their product.

Expected function signature:

```python
def multiply(a: int, b: int) -> int:
    ...
```

## 59. Exercise 2: Union

Write a function that accepts either:

- An integer user ID.
- A string user ID.

Use `isinstance()` to determine which type was supplied.

## 60. Exercise 3: Optional

Write a function that accepts a username that may be `None`.

If the username exists, print it.

Otherwise, print `"No username supplied"`.

## 61. Exercise 4: TypedDict

Create a TypedDict called `Product` containing:

- `id`: integer.
- `name`: string.
- `price`: float.
- `available`: boolean.

Create a list containing three products.

## 62. Exercise 5: Nested TypedDict

Create:

```python
class ReviewData(TypedDict):
    ...
```

And:

```python
class ProductData(TypedDict):
    ...
```

Each product should contain a list of reviews.

## 63. Exercise 6: Callable

Create a function that accepts another function.

The supplied function must:

- Accept two integers.
- Return an integer.

Pass an addition function to it.

## 64. Exercise 7: Iterable

Create a function that accepts an iterable of strings.

Test it with:

- A list.
- A tuple.
- A set.
- A generator.

## 65. Exercise 8: Iterator

Create your own iterator class that counts from 1 to 10.

Implement:

- `__iter__()`
- `__next__()`

Raise `StopIteration` when the counter reaches its limit.

## 66. Exercise 9: Protocol

Create a protocol called `PaymentProcessor`.

It should define a method:

```python
process_payment(amount: float) -> bool
```

Create two unrelated classes that implement this method.

Write a function that accepts the protocol.

## 67. Exercise 10: Generic

Create a generic class called `Container`.

It should:

- Accept a value of any specified type.
- Store that value.
- Return the stored value through a method.

Test it with strings and integers.

---

# PART 26 — QUICK REFERENCE

## 68. Important typing members

| Member | Meaning |
|---|---|
| `Any` | Unrestricted type |
| `Union` | One of several types |
| `Optional` | A type or None |
| `Literal` | Restricts values to specified choices |
| `TypedDict` | Describes dictionary structure |
| `Required` | Marks a TypedDict key as required |
| `NotRequired` | Marks a TypedDict key as optional |
| `ReadOnly` | Read-only TypedDict item |
| `Callable` | Describes callable signatures |
| `Iterable` | Describes iterable objects |
| `Iterator` | Describes iterator objects |
| `Sequence` | Describes ordered sequence interfaces |
| `Mapping` | Describes read-oriented mapping interfaces |
| `MutableMapping` | Describes mutable mappings |
| `Protocol` | Describes structural interfaces |
| `TypeVar` | Creates type variables |
| `Generic` | Creates generic classes |
| `NamedTuple` | Creates named immutable tuples |
| `ClassVar` | Marks class variables |
| `Final` | Indicates values that should not be reassigned |
| `NoReturn` | Function does not return normally |
| `Never` | Bottom type |
| `Self` | Current class type |
| `TypeGuard` | Custom type narrowing |
| `TypeIs` | Subtype-based type narrowing |
| `Annotated` | Attaches metadata to a type |
| `ParamSpec` | Captures callable parameter types |
| `Concatenate` | Combines callable parameter specifications |
| `Unpack` | Expands typed arguments |
| `TypeAlias` | Declares a type alias |
| `cast` | Informs the type checker of a type |
| `reveal_type` | Reveals inferred types |
| `assert_type` | Checks inferred types |
| `get_type_hints` | Retrieves annotations |
| `get_origin` | Retrieves a type expression's origin |
| `get_args` | Retrieves a type expression's arguments |

## 69. Recommended learning order

Study the concepts in this order:

1. Basic Python types.
2. Variables and function annotations.
3. Static type checking.
4. `Any`, `Union`, `Optional`, and `Literal`.
5. Collection annotations.
6. `TypedDict`.
7. `Callable`.
8. Iteration protocol.
9. `Iterable` and `Iterator`.
10. `Sequence` and `Mapping`.
11. `Protocol`.
12. `TypeVar`.
13. `Generic`.
14. `Annotated`.
15. `TypeGuard` and `TypeIs`.
16. Advanced callable typing.
17. Type introspection.

Do not rush through the concepts. Understand the problem each one solves before moving to the next.

---

# 70. Official Documentation

For further reading:

- Python typing documentation: https://docs.python.org/3/library/typing.html
- Python collections.abc: https://docs.python.org/3/library/collections.abc.html
- Python typing specification: https://typing.python.org/en/latest/
- Pydantic documentation: https://docs.pydantic.dev/
- FastAPI documentation: https://fastapi.tiangolo.com/

**Final advice:** Type hints are not just about telling Python whether something is an integer or string. They help describe relationships between data, functions, objects, and interfaces. Once you understand those relationships, advanced typing becomes much easier.
