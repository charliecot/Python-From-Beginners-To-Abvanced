# 🐍 PYTHON COMPLETE REFERENCE

## Beginner → Intermediate → Advanced

> A practical Python reference covering syntax, data types, methods, functions, OOP, dates/time, files, OS, errors, modules, APIs, databases, testing, concurrency, and advanced Python.

---

# TABLE OF CONTENTS

1. Python Basics
2. Variables
3. Comments
4. Data Types
5. Numbers
6. Strings
7. Booleans
8. None
9. Lists
10. Tuples
11. Sets
12. Dictionaries
13. Type Conversion
14. Operators
15. Conditional Statements
16. Loops
17. Functions
18. `*args` and `**kwargs`
19. Lambda Functions
20. Comprehensions
21. Scope
22. Dates and Time
23. Files
24. Directories
25. `os` Module
26. `pathlib`
27. Environment Variables
28. JSON
29. CSV
30. Exceptions and Error Handling
31. Custom Exceptions
32. Modules
33. Packages
34. Virtual Environments
35. Pip
36. Object-Oriented Programming
37. Inheritance
38. Encapsulation
39. Polymorphism
40. Abstract Classes
41. Properties
42. Magic/Dunder Methods
43. Dataclasses
44. Iterators
45. Generators
46. Decorators
47. Closures
48. Regular Expressions
49. Type Hints
50. Enums
51. Collections
52. Functional Programming
53. Copying Objects
54. Serialization
55. HTTP Requests/APIs
56. Working With Databases
57. Logging
58. Testing
59. Mocking
60. Concurrency
61. Threading
62. Multiprocessing
63. Async/Await
64. Context Managers
65. Memory Management
66. Python Security Basics
67. Project Structure
68. Useful Python Commands
69. Common Python Patterns
70. Beginner → Advanced Learning Path

---

# 1. PYTHON BASICS

Python is a high-level, interpreted, dynamically typed programming language.

Example:

```python
print("Hello World")
```

Output:

```text
Hello World
```

Run a Python file:

```bash
python main.py
```

Check Python version:

```bash
python --version
```

or:

```bash
python -V
```

Open Python interactive shell:

```bash
python
```

Exit:

```python
exit()
```

---

# 2. VARIABLES

A variable stores a reference to a value.

```python
name = "Nzegge"
age = 25
price = 100.50
is_active = True
```

Python does not require you to declare the type:

```python
name = "Charles"
```

Python determines the type automatically.

Check type:

```python
type(name)
```

Example:

```python
x = 10

print(type(x))
```

Output:

```text
<class 'int'>
```

Multiple assignment:

```python
a, b, c = 10, 20, 30
```

Same value:

```python
a = b = c = 100
```

Swap variables:

```python
a = 10
b = 20

a, b = b, a
```

---

# 3. COMMENTS

Single-line comment:

```python
# This is a comment
```

Multi-line documentation string:

```python
"""
This is a
multi-line string.
"""
```

Function documentation:

```python
def add(a, b):
    """Return the sum of two numbers."""
    return a + b
```

---

# 4. PYTHON DATA TYPES

Major built-in types:

```text
int
float
complex
bool
str
list
tuple
set
frozenset
dict
NoneType
bytes
bytearray
range
```

Check a type:

```python
type(value)
```

Check whether something belongs to a type:

```python
isinstance(value, int)
```

Example:

```python
age = 25

print(isinstance(age, int))
```

---

# 5. NUMBERS

## 5.1 INTEGER

```python
age = 25
```

Type:

```python
int
```

Methods/functions commonly used:

```python
abs()
pow()
round()
divmod()
```

Examples:

```python
abs(-10)
# 10

pow(2, 3)
# 8

round(10.567, 2)
# 10.57

divmod(10, 3)
# (3, 1)
```

Integer conversion:

```python
int("100")
int(10.5)
```

---

# 5.2 FLOAT

```python
price = 99.99
```

Convert:

```python
float("10.5")
```

Useful functions:

```python
abs()
round()
```

Example:

```python
price = 10.5678

print(round(price, 2))
```

---

# 5.3 COMPLEX

```python
z = 3 + 4j
```

Access parts:

```python
z.real
z.imag
```

Example:

```python
print(z.real)
print(z.imag)
```

---

# 6. STRINGS

A string stores text.

```python
name = "Nzegge"
```

Single quotes:

```python
name = 'Nzegge'
```

Double quotes:

```python
name = "Nzegge"
```

Triple quotes:

```python
message = """
Hello
Python
"""
```

---

# STRING INDEXING

```python
text = "Python"
```

Positions:

```text
P y t h o n
0 1 2 3 4 5
```

Access:

```python
text[0]
# P

text[2]
# t
```

Negative indexing:

```python
text[-1]
# n
```

---

# STRING SLICING

```python
text[start:stop:step]
```

Examples:

```python
text = "Python"

text[0:3]
# Pyt

text[:3]
# Pyt

text[3:]
# hon

text[:]
# Python

text[::-1]
# nohtyP
```

---

# STRING METHODS

## lower()

```python
"HELLO".lower()
```

Output:

```text
hello
```

## upper()

```python
"hello".upper()
```

## capitalize()

```python
"python".capitalize()
```

## title()

```python
"hello world".title()
```

## swapcase()

```python
"Hello".swapcase()
```

## casefold()

Useful for stronger case-insensitive comparisons:

```python
"HELLO".casefold()
```

---

# REMOVE SPACES

```python
text.strip()
```

Remove left spaces:

```python
text.lstrip()
```

Remove right spaces:

```python
text.rstrip()
```

---

# SEARCHING STRINGS

```python
text.find("Python")
```

Returns index or `-1`.

```python
text.index("Python")
```

Raises an exception if not found.

Check beginning:

```python
text.startswith("Py")
```

Check ending:

```python
text.endswith("on")
```

---

# REPLACING

```python
text.replace("Python", "Django")
```

Example:

```python
message = "I like Python"

message = message.replace("Python", "Django")
```

---

# SPLITTING

```python
text.split()
```

Example:

```python
text = "Python Django React"

text.split()
```

Result:

```python
["Python", "Django", "React"]
```

With separator:

```python
"a,b,c".split(",")
```

---

# JOINING

```python
",".join(["Python", "Django", "React"])
```

Result:

```text
Python,Django,React
```

---

# STRING CHECK METHODS

```python
text.isalpha()
text.isdigit()
text.isalnum()
text.isdecimal()
text.isnumeric()
text.islower()
text.isupper()
text.isspace()
text.istitle()
text.isidentifier()
```

Example:

```python
"123".isdigit()
# True
```

---

# STRING COUNT

```python
"banana".count("a")
```

Result:

```text
3
```

---

# STRING FORMATTING

## f-string

Preferred modern method:

```python
name = "Nzegge"
age = 25

print(f"My name is {name} and I am {age}.")
```

Expressions:

```python
print(f"Total: {10 + 20}")
```

Formatting numbers:

```python
price = 10.56789

print(f"{price:.2f}")
```

---

# 7. BOOLEAN

Boolean values:

```python
True
False
```

Example:

```python
is_logged_in = True
```

Boolean conversion:

```python
bool(1)
# True

bool(0)
# False

bool("")
# False

bool("hello")
# True
```

Falsy values include:

```text
False
None
0
0.0
""
[]
()
{}
set()
```

---

# 8. NONE

`None` represents absence of a value.

```python
result = None
```

Check:

```python
if result is None:
    print("No value")
```

Prefer:

```python
is None
```

instead of:

```python
== None
```

---

# 9. LIST

Lists are ordered and mutable.

```python
fruits = ["apple", "banana", "orange"]
```

Access:

```python
fruits[0]
```

Change:

```python
fruits[0] = "mango"
```

---

# LIST METHODS

## append()

Add one item:

```python
fruits.append("grape")
```

## extend()

Add multiple items:

```python
fruits.extend(["pear", "melon"])
```

## insert()

Insert at position:

```python
fruits.insert(1, "kiwi")
```

## remove()

Remove by value:

```python
fruits.remove("apple")
```

Raises `ValueError` if not found.

## pop()

Remove by index:

```python
fruits.pop()
```

or:

```python
fruits.pop(0)
```

Returns the removed item.

## clear()

```python
fruits.clear()
```

## index()

```python
fruits.index("banana")
```

## count()

```python
fruits.count("apple")
```

## sort()

```python
numbers = [3, 1, 2]

numbers.sort()
```

Descending:

```python
numbers.sort(reverse=True)
```

## reverse()

```python
numbers.reverse()
```

## copy()

```python
new_list = numbers.copy()
```

---

# LIST SLICING

```python
numbers = [0, 1, 2, 3, 4, 5]

numbers[1:4]
numbers[:3]
numbers[3:]
numbers[::-1]
```

---

# 10. TUPLE

Tuples are ordered and immutable.

```python
coordinates = (10, 20)
```

Single-item tuple:

```python
value = (10,)
```

Methods:

```python
count()
index()
```

Example:

```python
numbers = (1, 2, 2, 3)

numbers.count(2)
numbers.index(3)
```

Tuple unpacking:

```python
person = ("Nzegge", 25)

name, age = person
```

---

# 11. SET

A set stores unique values.

```python
numbers = {1, 2, 3, 3}

print(numbers)
```

Duplicate `3` is removed.

---

# SET METHODS

```python
add()
remove()
discard()
pop()
clear()
copy()
update()
```

Set operations:

```python
union()
intersection()
difference()
symmetric_difference()
```

Example:

```python
a = {1, 2, 3}
b = {3, 4, 5}

a.union(b)
a.intersection(b)
a.difference(b)
a.symmetric_difference(b)
```

Operators:

```python
a | b
a & b
a - b
a ^ b
```

Subset:

```python
a.issubset(b)
```

Superset:

```python
a.issuperset(b)
```

Disjoint:

```python
a.isdisjoint(b)
```

---

# 12. FROZENSET

Immutable set:

```python
numbers = frozenset([1, 2, 3])
```

Useful when a set needs to be hashable.

---

# 13. DICTIONARY

Dictionary stores key/value pairs.

```python
user = {
    "name": "Nzegge",
    "age": 25,
    "active": True
}
```

Access:

```python
user["name"]
```

Safer:

```python
user.get("name")
```

---

# DICTIONARY METHODS

## keys()

```python
user.keys()
```

## values()

```python
user.values()
```

## items()

```python
user.items()
```

## get()

```python
user.get("email")
```

Default:

```python
user.get("email", "No email")
```

## update()

```python
user.update({
    "age": 26,
    "email": "example@gmail.com"
})
```

## pop()

```python
user.pop("age")
```

## popitem()

```python
user.popitem()
```

Removes the last inserted key/value pair.

## setdefault()

```python
user.setdefault("country", "Cameroon")
```

## clear()

```python
user.clear()
```

## copy()

```python
new_user = user.copy()
```

---

# DICTIONARY UNPACKING

```python
user = {
    "name": "Nzegge",
    "age": 25
}

print(user["name"])
```

Merge dictionaries:

```python
a = {"name": "Nzegge"}
b = {"age": 25}

result = {**a, **b}
```

Modern:

```python
result = a | b
```

---

# 14. TYPE CONVERSION

String:

```python
str(100)
```

Integer:

```python
int("100")
```

Float:

```python
float("10.5")
```

Boolean:

```python
bool(1)
```

List:

```python
list((1, 2, 3))
```

Tuple:

```python
tuple([1, 2, 3])
```

Set:

```python
set([1, 2, 2, 3])
```

Dictionary:

```python
dict(name="Nzegge", age=25)
```

---

# 15. OPERATORS

## Arithmetic

```python
+
-
*
/
//
%
**
```

Example:

```python
10 / 3
# 3.333...

10 // 3
# 3

10 % 3
# 1

2 ** 3
# 8
```

## Comparison

```python
==
!=
>
<
>=
<=
```

## Logical

```python
and
or
not
```

## Membership

```python
in
not in
```

Example:

```python
"python" in ["python", "django"]
```

## Identity

```python
is
is not
```

Important:

```python
a == b
```

means values are equal.

```python
a is b
```

means they refer to the same object.

---

# 16. CONDITIONAL STATEMENTS

```python
age = 20

if age >= 18:
    print("Adult")
elif age >= 13:
    print("Teenager")
else:
    print("Child")
```

---

# TERNARY OPERATOR

```python
status = "Adult" if age >= 18 else "Minor"
```

---

# MATCH / CASE

```python
status = "pending"

match status:
    case "pending":
        print("Waiting")
    case "completed":
        print("Done")
    case "cancelled":
        print("Cancelled")
    case _:
        print("Unknown")
```

---

# 17. LOOPS

## FOR LOOP

```python
for number in [1, 2, 3]:
    print(number)
```

## RANGE

```python
for i in range(5):
    print(i)
```

Result:

```text
0
1
2
3
4
```

Start/end:

```python
range(1, 6)
```

Step:

```python
range(0, 10, 2)
```

Reverse:

```python
range(10, 0, -1)
```

---

# WHILE

```python
count = 0

while count < 5:
    print(count)
    count += 1
```

---

# BREAK

```python
for i in range(10):
    if i == 5:
        break
```

---

# CONTINUE

```python
for i in range(10):
    if i % 2 == 0:
        continue

    print(i)
```

---

# PASS

```python
if True:
    pass
```

Used as a placeholder.

---

# 18. FUNCTIONS

Basic:

```python
def greet():
    print("Hello")
```

Call:

```python
greet()
```

Parameters:

```python
def greet(name):
    print(f"Hello {name}")
```

Return:

```python
def add(a, b):
    return a + b
```

Default parameter:

```python
def greet(name="Guest"):
    print(name)
```

Keyword arguments:

```python
greet(name="Nzegge")
```

---

# FUNCTION TYPE HINTS

```python
def add(a: int, b: int) -> int:
    return a + b
```

---

# DOCSTRINGS

```python
def add(a, b):
    """
    Add two numbers.

    Returns:
        int: Sum of the numbers.
    """
    return a + b
```

---

# 19. *args

Allows multiple positional arguments.

```python
def add(*numbers):
    return sum(numbers)

print(add(1, 2, 3, 4))
```

Inside the function:

```python
numbers
```

is a tuple.

---

# 20. **kwargs

Allows multiple keyword arguments.

```python
def show_user(**user):
    print(user)

show_user(
    name="Nzegge",
    age=25
)
```

Inside:

```python
user
```

is a dictionary.

---

# Combining

```python
def function(a, b, *args, **kwargs):
    pass
```

Typical order:

```text
normal parameters
*args
**kwargs
```

---

# 21. LAMBDA

Small anonymous function:

```python
square = lambda x: x ** 2

print(square(5))
```

Useful with:

```python
map()
filter()
sorted()
```

Example:

```python
numbers = [1, 2, 3, 4]

squares = list(map(lambda x: x * x, numbers))
```

---

# 22. COMPREHENSIONS

## List comprehension

```python
numbers = [1, 2, 3, 4]

squares = [x ** 2 for x in numbers]
```

With condition:

```python
even = [x for x in numbers if x % 2 == 0]
```

## Dictionary comprehension

```python
squares = {
    x: x ** 2
    for x in range(5)
}
```

## Set comprehension

```python
values = {x % 3 for x in range(10)}
```

## Generator expression

```python
values = (x ** 2 for x in range(10))
```

---

# 23. SCOPE

Python LEGB rule:

```text
L = Local
E = Enclosing
G = Global
B = Built-in
```

Example:

```python
x = 10

def test():
    x = 20
    print(x)
```

Global:

```python
x = 10

def change():
    global x
    x = 20
```

For enclosing scope:

```python
def outer():
    x = 10

    def inner():
        nonlocal x
        x = 20

    inner()
    return x
```

---

# 24. DATES AND TIME

Import:

```python
import datetime
```

Or:

```python
from datetime import date, datetime, time, timedelta
```

---

# CURRENT DATE

```python
from datetime import date

today = date.today()

print(today)
```

---

# CURRENT DATETIME

```python
from datetime import datetime

now = datetime.now()

print(now)
```

---

# TIME

```python
from datetime import time

t = time(14, 30, 0)

print(t)
```

---

# DATE COMPONENTS

```python
today.year
today.month
today.day
today.weekday()
today.isoweekday()
```

`weekday()`:

```text
Monday = 0
Sunday = 6
```

`isoweekday()`:

```text
Monday = 1
Sunday = 7
```

---

# DATETIME COMPONENTS

```python
now.year
now.month
now.day
now.hour
now.minute
now.second
now.microsecond
```

---

# CREATE DATE

```python
d = date(2026, 10, 2)
```

---

# CREATE DATETIME

```python
dt = datetime(
    2026,
    10,
    2,
    14,
    30
)
```

---

# TIMESTAMP

Convert datetime to timestamp:

```python
timestamp = datetime.now().timestamp()
```

Convert timestamp:

```python
datetime.fromtimestamp(timestamp)
```

---

# FORMAT DATE

Use `strftime()`:

```python
now.strftime("%Y-%m-%d")
```

Example:

```python
now.strftime("%d/%m/%Y")
```

Common format codes:

```text
%Y = four-digit year
%y = two-digit year
%m = month
%d = day
%H = hour 24-hour
%I = hour 12-hour
%M = minute
%S = second
%f = microsecond
%A = full weekday
%a = short weekday
%B = full month
%b = short month
```

Example:

```python
now.strftime("%A, %d %B %Y")
```

---

# PARSE STRING INTO DATE

Use `strptime()`:

```python
date_string = "2026-10-02"

dt = datetime.strptime(
    date_string,
    "%Y-%m-%d"
)
```

---

# TIME DELTAS

```python
from datetime import timedelta

tomorrow = today + timedelta(days=1)

yesterday = today - timedelta(days=1)
```

Hours:

```python
future = now + timedelta(hours=5)
```

Minutes:

```python
future = now + timedelta(minutes=30)
```

---

# DATE DIFFERENCE

```python
date1 = date(2026, 10, 10)
date2 = date(2026, 10, 2)

difference = date1 - date2

print(difference.days)
```

---

# TIME ZONES

Modern Python should use timezone-aware datetimes.

```python
from datetime import datetime, timezone

now = datetime.now(timezone.utc)
```

---

# 25. FILE HANDLING

Open a file:

```python
file = open("data.txt")
```

Better:

```python
with open("data.txt", "r") as file:
    content = file.read()
```

The `with` statement automatically closes the file.

---

# FILE MODES

```text
r   read
w   write
a   append
x   create
b   binary
t   text
+   read/write
```

Examples:

```python
open("file.txt", "r")
open("file.txt", "w")
open("file.txt", "a")
open("image.jpg", "rb")
```

---

# READ ENTIRE FILE

```python
with open("data.txt", "r") as file:
    content = file.read()
```

---

# READ ONE LINE

```python
with open("data.txt") as file:
    line = file.readline()
```

---

# READ ALL LINES

```python
with open("data.txt") as file:
    lines = file.readlines()
```

---

# LOOP THROUGH FILE

```python
with open("data.txt") as file:
    for line in file:
        print(line)
```

---

# WRITE FILE

```python
with open("data.txt", "w") as file:
    file.write("Hello Python")
```

WARNING:

`w` replaces existing content.

---

# APPEND

```python
with open("data.txt", "a") as file:
    file.write("\nNew line")
```

---

# WRITE MULTIPLE LINES

```python
lines = [
    "Python\n",
    "Django\n",
    "React\n"
]

with open("data.txt", "w") as file:
    file.writelines(lines)
```

---

# FILE ENCODING

Recommended:

```python
with open(
    "data.txt",
    "r",
    encoding="utf-8"
) as file:
    content = file.read()
```

---

# BINARY FILES

Images, PDFs, etc.:

```python
with open("image.jpg", "rb") as file:
    data = file.read()
```

Write binary:

```python
with open("copy.jpg", "wb") as file:
    file.write(data)
```

---

# FILE POINTER

```python
file.tell()
```

Move pointer:

```python
file.seek(0)
```

---

# 26. OS MODULE

Import:

```python
import os
```

Current directory:

```python
os.getcwd()
```

Change directory:

```python
os.chdir("folder")
```

List files:

```python
os.listdir()
```

Create directory:

```python
os.mkdir("test")
```

Create nested directories:

```python
os.makedirs("a/b/c")
```

Remove empty directory:

```python
os.rmdir("test")
```

Remove nested empty directories:

```python
os.removedirs("a/b/c")
```

Rename:

```python
os.rename("old.txt", "new.txt")
```

Delete:

```python
os.remove("file.txt")
```

Check existence:

```python
os.path.exists("file.txt")
```

Check file:

```python
os.path.isfile("file.txt")
```

Check directory:

```python
os.path.isdir("folder")
```

Join paths:

```python
os.path.join("folder", "file.txt")
```

Absolute path:

```python
os.path.abspath("file.txt")
```

File size:

```python
os.path.getsize("file.txt")
```

Split path:

```python
os.path.split(path)
```

File extension:

```python
os.path.splitext("image.jpg")
```

---

# 27. PATHLIB

`pathlib` is usually cleaner than manually building paths with `os.path`.

```python
from pathlib import Path
```

Current directory:

```python
Path.cwd()
```

Create path:

```python
path = Path("data/file.txt")
```

Check:

```python
path.exists()
path.is_file()
path.is_dir()
```

Read:

```python
content = path.read_text()
```

Write:

```python
path.write_text("Hello")
```

Create directory:

```python
Path("data").mkdir()
```

Nested:

```python
Path("a/b/c").mkdir(
    parents=True,
    exist_ok=True
)
```

File name:

```python
path.name
```

Extension:

```python
path.suffix
```

Parent:

```python
path.parent
```

Stem:

```python
path.stem
```

Join:

```python
Path("data") / "file.txt"
```

Find files:

```python
Path(".").glob("*.txt")
```

Recursive:

```python
Path(".").rglob("*.py")
```

---

# 28. ENVIRONMENT VARIABLES

Use:

```python
import os
```

Get variable:

```python
os.getenv("SECRET_KEY")
```

With default:

```python
os.getenv("PORT", "8000")
```

Set:

```python
os.environ["MODE"] = "development"
```

Check:

```python
"PATH" in os.environ
```

For real projects, secrets should normally be stored outside source code.

Example:

```text
SECRET_KEY=abc123
DEBUG=True
DATABASE_URL=...
```

---

# 29. JSON

Import:

```python
import json
```

Python dictionary → JSON string:

```python
data = {
    "name": "Nzegge",
    "age": 25
}

json_string = json.dumps(data)
```

Pretty JSON:

```python
json.dumps(
    data,
    indent=4
)
```

JSON string → Python:

```python
data = json.loads(json_string)
```

Write JSON file:

```python
with open("data.json", "w") as file:
    json.dump(data, file, indent=4)
```

Read JSON:

```python
with open("data.json") as file:
    data = json.load(file)
```

---

# 30. CSV

```python
import csv
```

Write CSV:

```python
rows = [
    ["name", "age"],
    ["Nzegge", 25]
]

with open(
    "users.csv",
    "w",
    newline="",
    encoding="utf-8"
) as file:

    writer = csv.writer(file)

    writer.writerows(rows)
```

Read:

```python
with open("users.csv", encoding="utf-8") as file:
    reader = csv.reader(file)

    for row in reader:
        print(row)
```

Dictionary CSV:

```python
with open("users.csv", encoding="utf-8") as file:
    reader = csv.DictReader(file)

    for row in reader:
        print(row["name"])
```

---

# 31. EXCEPTION HANDLING

Basic:

```python
try:
    number = int(input("Enter number: "))
except ValueError:
    print("Invalid number")
```

Multiple exceptions:

```python
try:
    ...
except ValueError:
    ...
except TypeError:
    ...
```

Generic:

```python
except Exception as error:
    print(error)
```

---

# ELSE

```python
try:
    result = 10 / 2
except ZeroDivisionError:
    print("Cannot divide")
else:
    print(result)
```

`else` executes if no exception occurred.

---

# FINALLY

```python
try:
    file = open("data.txt")
except FileNotFoundError:
    print("Missing")
finally:
    print("Finished")
```

`finally` normally runs regardless of success/failure.

---

# RAISE

```python
age = -1

if age < 0:
    raise ValueError("Age cannot be negative")
```

---

# ASSERT

```python
age = 20

assert age >= 18
```

With message:

```python
assert age >= 18, "User must be an adult"
```

Do not use assertions as your main runtime validation mechanism.

---

# 32. CUSTOM EXCEPTIONS

```python
class InsufficientBalanceError(Exception):
    pass
```

Use:

```python
raise InsufficientBalanceError(
    "Not enough balance"
)
```

---

# 33. MODULES

A module is normally a `.py` file.

Example:

```text
math_utils.py
```

```python
def add(a, b):
    return a + b
```

Import:

```python
import math_utils

math_utils.add(2, 3)
```

Specific import:

```python
from math_utils import add
```

Alias:

```python
import math_utils as mathu
```

---

# **name**

```python
if __name__ == "__main__":
    print("Program started")
```

This allows code to run when the file is executed directly but not when imported.

---

# 34. PACKAGES

Example:

```text
myproject/
    package/
        __init__.py
        users.py
        products.py
```

Import:

```python
from package.users import User
```

---

# 35. VIRTUAL ENVIRONMENTS

Create:

```bash
python -m venv venv
```

Windows activation CMD:

```bash
venv\Scripts\activate
```

PowerShell:

```powershell
venv\Scripts\Activate.ps1
```

Git Bash:

```bash
source venv/Scripts/activate
```

Deactivate:

```bash
deactivate
```

---

# 36. PIP

Install:

```bash
pip install requests
```

Specific version:

```bash
pip install requests==2.32.0
```

Upgrade:

```bash
pip install --upgrade requests
```

Uninstall:

```bash
pip uninstall requests
```

List packages:

```bash
pip list
```

Show package:

```bash
pip show requests
```

Save dependencies:

```bash
pip freeze > requirements.txt
```

Install requirements:

```bash
pip install -r requirements.txt
```

---

# 37. OBJECT-ORIENTED PROGRAMMING

A class defines objects.

```python
class User:
    pass
```

Create object:

```python
user = User()
```

---

# **init**

```python
class User:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Create:

```python
user = User("Nzegge", 25)
```

Access:

```python
print(user.name)
```

---

# INSTANCE METHODS

```python
class User:

    def __init__(self, name):
        self.name = name

    def greet(self):
        return f"Hello {self.name}"
```

Call:

```python
user.greet()
```

---

# CLASS VARIABLES

```python
class User:

    country = "Cameroon"
```

---

# CLASS METHODS

```python
class User:

    count = 0

    @classmethod
    def get_count(cls):
        return cls.count
```

---

# STATIC METHODS

```python
class Math:

    @staticmethod
    def add(a, b):
        return a + b
```

Call:

```python
Math.add(2, 3)
```

---

# 38. INHERITANCE

```python
class Animal:

    def speak(self):
        print("Animal sound")


class Dog(Animal):

    def bark(self):
        print("Woof")
```

Use:

```python
dog = Dog()

dog.speak()
dog.bark()
```

---

# SUPER()

```python
class Animal:

    def __init__(self, name):
        self.name = name


class Dog(Animal):

    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed
```

---

# 39. POLYMORPHISM

Different classes can provide the same method.

```python
class Dog:

    def speak(self):
        return "Woof"


class Cat:

    def speak(self):
        return "Meow"
```

```python
animals = [Dog(), Cat()]

for animal in animals:
    print(animal.speak())
```

---

# 40. ENCAPSULATION

Python uses naming conventions.

Public:

```python
self.name
```

Internal/non-public convention:

```python
self._name
```

Name mangling:

```python
self.__password
```

---

# 41. PROPERTY

```python
class User:

    def __init__(self, age):
        self._age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("Invalid age")

        self._age = value
```

Use:

```python
user.age = 25
print(user.age)
```

---

# 42. MAGIC / DUNDER METHODS

Special methods use double underscores.

Examples:

```python
__init__
__str__
__repr__
__len__
__eq__
__lt__
__gt__
__add__
__sub__
__mul__
__iter__
__next__
__enter__
__exit__
```

---

# **str**

```python
class User:

    def __init__(self, name):
        self.name = name

    def __str__(self):
        return self.name
```

---

# **repr**

Used for developer-oriented representation:

```python
def __repr__(self):
    return f"User(name={self.name!r})"
```

---

# OPERATOR OVERLOADING

```python
class Number:

    def __init__(self, value):
        self.value = value

    def __add__(self, other):
        return Number(
            self.value + other.value
        )
```

---

# 43. DATACLASS

Useful for classes mainly storing data.

```python
from dataclasses import dataclass

@dataclass
class User:
    name: str
    age: int
```

Create:

```python
user = User("Nzegge", 25)
```

Dataclasses automatically provide useful methods such as initialization and representation.

---

# 44. ITERATORS

An iterable can produce values.

Examples:

```python
list
tuple
set
dict
str
range
```

Get iterator:

```python
numbers = iter([1, 2, 3])
```

Get next:

```python
next(numbers)
```

Repeated:

```python
next(numbers)
next(numbers)
next(numbers)
```

Eventually `StopIteration` is raised.

---

# CUSTOM ITERATOR

```python
class Counter:

    def __init__(self, maximum):
        self.current = 0
        self.maximum = maximum

    def __iter__(self):
        return self

    def __next__(self):

        if self.current >= self.maximum:
            raise StopIteration

        value = self.current
        self.current += 1

        return value
```

---

# 45. GENERATORS

Generator uses `yield`.

```python
def numbers():
    yield 1
    yield 2
    yield 3
```

Use:

```python
for number in numbers():
    print(number)
```

Generators are useful for large data because values can be produced lazily.

---

# GENERATOR EXPRESSION

```python
squares = (
    x ** 2
    for x in range(1000000)
)
```

---

# 46. DECORATORS

A decorator modifies or wraps a function.

```python
def logger(function):

    def wrapper():
        print("Before")
        function()
        print("After")

    return wrapper
```

Use:

```python
@logger
def hello():
    print("Hello")
```

---

# DECORATOR WITH ARGUMENTS

Use `*args` and `**kwargs`:

```python
def logger(function):

    def wrapper(*args, **kwargs):

        print("Calling function")

        result = function(*args, **kwargs)

        print("Finished")

        return result

    return wrapper
```

Better:

```python
from functools import wraps

def logger(function):

    @wraps(function)
    def wrapper(*args, **kwargs):
        print("Calling")
        return function(*args, **kwargs)

    return wrapper
```

---

# 47. CLOSURES

A nested function remembers variables from its enclosing function.

```python
def multiplier(x):

    def multiply(y):
        return x * y

    return multiply
```

Use:

```python
double = multiplier(2)

print(double(5))
```

Result:

```text
10
```

---

# 48. REGULAR EXPRESSIONS

Import:

```python
import re
```

Search:

```python
re.search(pattern, text)
```

Match beginning:

```python
re.match(pattern, text)
```

Find all:

```python
re.findall(pattern, text)
```

Replace:

```python
re.sub(pattern, replacement, text)
```

Split:

```python
re.split(pattern, text)
```

Example:

```python
text = "My phone is 12345"

numbers = re.findall(
    r"\d+",
    text
)
```

---

# COMMON REGEX SYMBOLS

```text
\d   digit
\D   non-digit
\w   word character
\W   non-word
\s   whitespace
\S   non-whitespace
.    any character
^    beginning
$    end
+    one or more
*    zero or more
?    zero or one
{n}  exactly n
{n,m} between n and m
[]   character set
()   group
|    OR
```

---

# 49. TYPE HINTS

Basic:

```python
name: str
age: int
price: float
active: bool
```

Function:

```python
def greet(name: str) -> str:
    return f"Hello {name}"
```

List:

```python
numbers: list[int]
```

Dictionary:

```python
users: dict[str, int]
```

Optional value:

```python
str | None
```

Example:

```python
def get_name() -> str | None:
    return None
```

---

# TYPE ALIASES

```python
UserID = int
```

---

# 50. ENUM

```python
from enum import Enum

class Status(Enum):
    PENDING = "pending"
    COMPLETED = "completed"
    CANCELLED = "cancelled"
```

Use:

```python
Status.PENDING
Status.PENDING.value
```

---

# 51. COLLECTIONS

Python provides specialized containers.

```python
from collections import (
    Counter,
    defaultdict,
    deque,
    namedtuple
)
```

---

# COUNTER

```python
from collections import Counter

letters = Counter("banana")

print(letters)
```

Most common:

```python
letters.most_common(2)
```

---

# DEFAULTDICT

```python
from collections import defaultdict

users = defaultdict(list)

users["admin"].append("Nzegge")
```

---

# DEQUE

Efficient queue:

```python
from collections import deque

queue = deque()

queue.append("A")
queue.append("B")

queue.popleft()
```

---

# 52. FUNCTIONAL PROGRAMMING

## map()

```python
numbers = [1, 2, 3]

result = list(
    map(lambda x: x * 2, numbers)
)
```

## filter()

```python
result = list(
    filter(lambda x: x > 2, numbers)
)
```

## reduce()

```python
from functools import reduce

result = reduce(
    lambda a, b: a + b,
    numbers
)
```

---

# SORTED

```python
numbers = [3, 1, 2]

sorted(numbers)
```

With key:

```python
users = [
    {"name": "A", "age": 30},
    {"name": "B", "age": 20}
]

sorted(
    users,
    key=lambda user: user["age"]
)
```

Descending:

```python
sorted(
    users,
    key=lambda user: user["age"],
    reverse=True
)
```

---

# 53. COPY

Assignment does not create a new object:

```python
a = [1, 2, 3]
b = a
```

Now both reference the same list.

Shallow copy:

```python
import copy

b = copy.copy(a)
```

Deep copy:

```python
b = copy.deepcopy(a)
```

Deep copy is important for nested mutable structures.

---

# 54. SERIALIZATION

Serialization means converting an object/data structure into a format that can be stored or transferred.

Common formats:

```text
JSON
pickle
CSV
```

JSON:

```python
json.dumps(data)
```

Pickle:

```python
import pickle
```

Serialize:

```python
with open("data.pkl", "wb") as file:
    pickle.dump(data, file)
```

Deserialize:

```python
with open("data.pkl", "rb") as file:
    data = pickle.load(file)
```

IMPORTANT:

Never load untrusted pickle files.

---

# 55. HTTP REQUESTS / APIs

The standard library provides:

```python
urllib
```

A common third-party package is:

```bash
pip install requests
```

Example:

```python
import requests

response = requests.get(
    "https://example.com"
)

print(response.status_code)
print(response.text)
```

JSON:

```python
response.json()
```

---

# POST REQUEST

```python
data = {
    "name": "Nzegge",
    "age": 25
}

response = requests.post(
    "https://example.com/api/users/",
    json=data
)
```

Headers:

```python
headers = {
    "Authorization": "Bearer TOKEN"
}

response = requests.get(
    url,
    headers=headers
)
```

Timeout:

```python
requests.get(
    url,
    timeout=10
)
```

Always consider using timeouts in production network requests.

---

# 56. DATABASES

Python supports database access through libraries.

SQLite is built into Python.

```python
import sqlite3
```

Connect:

```python
connection = sqlite3.connect(
    "database.db"
)
```

Cursor:

```python
cursor = connection.cursor()
```

Create table:

```python
cursor.execute("""
    CREATE TABLE IF NOT EXISTS users (
        id INTEGER PRIMARY KEY,
        name TEXT,
        age INTEGER
    )
""")
```

Insert:

```python
cursor.execute(
    "INSERT INTO users (name, age) VALUES (?, ?)",
    ("Nzegge", 25)
)
```

Commit:

```python
connection.commit()
```

Query:

```python
cursor.execute(
    "SELECT * FROM users"
)

rows = cursor.fetchall()
```

Close:

```python
connection.close()
```

IMPORTANT:

Use parameterized queries instead of constructing SQL with string concatenation.

---

# 57. LOGGING

Instead of relying only on `print()` in applications, use logging.

```python
import logging
```

Basic:

```python
logging.basicConfig(
    level=logging.INFO
)
```

Messages:

```python
logging.debug("Debug message")
logging.info("Information")
logging.warning("Warning")
logging.error("Error")
logging.critical("Critical")
```

Example:

```python
logger = logging.getLogger(__name__)

logger.info("User logged in")
```

---

# 58. TESTING

Python has built-in `unittest`.

```python
import unittest
```

Example:

```python
def add(a, b):
    return a + b
```

Test:

```python
class TestAdd(unittest.TestCase):

    def test_add(self):
        self.assertEqual(
            add(2, 3),
            5
        )
```

Run:

```bash
python -m unittest
```

---

# COMMON ASSERTIONS

```python
self.assertEqual(a, b)
self.assertNotEqual(a, b)

self.assertTrue(value)
self.assertFalse(value)

self.assertIs(a, b)
self.assertIsNone(value)

self.assertIn(a, b)
self.assertNotIn(a, b)

self.assertRaises(
    ValueError,
    function
)
```

---

# 59. MOCKING

Use:

```python
from unittest.mock import Mock
```

Example:

```python
mock = Mock()

mock.return_value = "Hello"

print(mock())
```

Patch:

```python
from unittest.mock import patch
```

Example:

```python
@patch("module.requests.get")
def test_api(mock_get):
    ...
```

Mocks are useful when testing code that communicates with:

```text
APIs
databases
email
external services
files
queues
```

---

# 60. CONCURRENCY

Python has several approaches:

```text
threading
multiprocessing
asyncio
concurrent.futures
```

Choose based on the workload.

Rough rule:

```text
I/O-bound → threading / asyncio
CPU-bound → multiprocessing
```

---

# 61. THREADING

```python
import threading

def task():
    print("Running")

thread = threading.Thread(
    target=task
)

thread.start()
thread.join()
```

Useful for I/O-heavy work.

---

# 62. MULTIPROCESSING

```python
from multiprocessing import Process

def task():
    print("Running")

process = Process(
    target=task
)

process.start()
process.join()
```

Processes have separate memory spaces and can use multiple CPU cores.

---

# 63. ASYNC / AWAIT

Define async function:

```python
async def fetch_data():
    ...
```

Await:

```python
result = await fetch_data()
```

Run:

```python
import asyncio

asyncio.run(fetch_data())
```

Example:

```python
import asyncio

async def task():
    await asyncio.sleep(1)
    return "Done"

result = asyncio.run(task())

print(result)
```

Useful for applications handling many I/O operations.

---

# 64. CONTEXT MANAGERS

The `with` statement is a context manager.

Example:

```python
with open("data.txt") as file:
    data = file.read()
```

Custom context manager:

```python
class MyContext:

    def __enter__(self):
        print("Enter")
        return self

    def __exit__(
        self,
        exc_type,
        exc_value,
        traceback
    ):
        print("Exit")
```

Use:

```python
with MyContext():
    print("Working")
```

---

# 65. MEMORY MANAGEMENT

Python manages memory automatically.

Important concepts:

```text
objects
references
reference counting
garbage collection
```

Check object identity:

```python
id(object)
```

Check reference count:

```python
import sys

sys.getrefcount(object)
```

Garbage collector:

```python
import gc

gc.collect()
```

You normally do not manually manage memory in ordinary Python applications.

---

# 66. PYTHON SECURITY BASICS

Never execute untrusted input using:

```python
eval()
```

or:

```python
exec()
```

Avoid shell injection.

Instead of:

```python
os.system(user_input)
```

prefer safer APIs such as `subprocess` with argument lists when a subprocess is genuinely required.

Do not store secrets directly in source code:

```python
# BAD
SECRET_KEY = "my-secret"
```

Prefer environment variables/configuration.

Validate user input.

Use parameterized SQL.

Do not deserialize untrusted pickle data.

Keep dependencies updated.

---

# 67. SUBPROCESS

Run external programs:

```python
import subprocess

result = subprocess.run(
    ["python", "--version"],
    capture_output=True,
    text=True
)

print(result.stdout)
```

Check failure:

```python
subprocess.run(
    command,
    check=True
)
```

---

# 68. SYS MODULE

```python
import sys
```

Python version:

```python
sys.version
```

Command-line arguments:

```python
sys.argv
```

Exit:

```python
sys.exit()
```

Python path:

```python
sys.path
```

---

# 69. MATH MODULE

```python
import math
```

Examples:

```python
math.sqrt(16)
math.ceil(4.2)
math.floor(4.9)
math.factorial(5)
math.gcd(12, 8)
math.pi
math.e
```

---

# 70. RANDOM

```python
import random
```

Random number:

```python
random.randint(1, 10)
```

Random float:

```python
random.random()
```

Choice:

```python
random.choice(["A", "B", "C"])
```

Shuffle:

```python
items = [1, 2, 3]

random.shuffle(items)
```

Sample:

```python
random.sample(items, 2)
```

Do not use `random` for security-sensitive tokens.

---

# 71. SECRETS

For security-sensitive random values:

```python
import secrets
```

Token:

```python
secrets.token_hex(32)
```

URL-safe token:

```python
secrets.token_urlsafe(32)
```

Random secure choice:

```python
secrets.choice(["A", "B", "C"])
```

Useful for:

```text
password reset tokens
temporary secrets
authentication tokens
secure random values
```

---

# 72. UUID

```python
import uuid
```

Generate:

```python
uuid.uuid4()
```

String:

```python
str(uuid.uuid4())
```

Useful for:

```text
IDs
orders
resources
file names
distributed systems
```

---

# 73. HASHING

```python
import hashlib
```

Example SHA-256:

```python
hashlib.sha256(
    b"hello"
).hexdigest()
```

IMPORTANT:

Do not use raw SHA-256 as a password-storage algorithm. Passwords should use dedicated password hashing algorithms/frameworks such as Argon2, bcrypt, scrypt, or PBKDF2.

---

# 74. ITERTOOLS

```python
import itertools
```

Useful functions:

```python
count()
cycle()
repeat()
chain()
product()
permutations()
combinations()
```

Example:

```python
list(
    itertools.combinations(
        [1, 2, 3],
        2
    )
)
```

---

# 75. FUNCTOOLS

```python
from functools import (
    reduce,
    partial,
    wraps,
    lru_cache
)
```

Memoization:

```python
from functools import lru_cache

@lru_cache
def fibonacci(n):
    if n < 2:
        return n

    return (
        fibonacci(n - 1)
        + fibonacci(n - 2)
    )
```

---

# 76. CACHING

Useful for expensive calculations.

```python
from functools import lru_cache

@lru_cache(maxsize=128)
def calculate(value):
    return value * value
```

---

# 77. ENUMERATE

Instead of:

```python
index = 0

for item in items:
    print(index, item)
    index += 1
```

Use:

```python
for index, item in enumerate(items):
    print(index, item)
```

Starting index:

```python
for index, item in enumerate(
    items,
    start=1
):
    print(index, item)
```

---

# 78. ZIP

Combine iterables:

```python
names = ["A", "B", "C"]
ages = [20, 25, 30]

for name, age in zip(names, ages):
    print(name, age)
```

Create dictionary:

```python
dict(zip(names, ages))
```

---

# 79. ANY AND ALL

`any()`:

```python
any([False, False, True])
```

Returns:

```text
True
```

`all()`:

```python
all([True, True, True])
```

Returns:

```text
True
```

Useful for validation:

```python
numbers = [2, 4, 6]

all(x % 2 == 0 for x in numbers)
```

---

# 80. MIN / MAX / SUM

```python
numbers = [1, 2, 3, 4]

min(numbers)
max(numbers)
sum(numbers)
```

---

# 81. LEN

```python
len("Python")
len([1, 2, 3])
len({"a": 1})
```

---

# 82. DIR

See attributes/methods:

```python
dir("hello")
```

Very useful while learning Python.

---

# 83. HELP

```python
help(str)
```

Specific method:

```python
help(str.split)
```

---

# 84. CALLABLE

Check whether something can be called:

```python
callable(print)
```

---

# 85. GLOBALS / LOCALS

```python
globals()
```

```python
locals()
```

Useful for introspection, but should not normally be used as a substitute for clear program structure.

---

# 86. OBJECT INTROSPECTION

```python
type(obj)
id(obj)
dir(obj)
hasattr(obj, "name")
getattr(obj, "name")
setattr(obj, "name", "Nzegge")
delattr(obj, "name")
```

Example:

```python
if hasattr(user, "name"):
    print(getattr(user, "name"))
```

---

# 87. PROPERTY-BASED OBJECT DESIGN

Example:

```python
class BankAccount:

    def __init__(self, balance):
        self._balance = balance

    @property
    def balance(self):
        return self._balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError(
                "Amount must be positive"
            )

        self._balance += amount
```

---

# 88. ABSTRACT BASE CLASSES

```python
from abc import ABC, abstractmethod

class Animal(ABC):

    @abstractmethod
    def speak(self):
        pass
```

Child class must implement:

```python
class Dog(Animal):

    def speak(self):
        return "Woof"
```

---

# 89. PROTOCOLS

Structural typing:

```python
from typing import Protocol

class Printable(Protocol):

    def print(self) -> None:
        ...
```

A class can satisfy the protocol by providing the required behavior without explicitly inheriting from it.

---

# 90. GENERIC TYPES

```python
from typing import TypeVar, Generic

T = TypeVar("T")
```

Example:

```python
def first(items: list[T]) -> T:
    return items[0]
```

Modern Python type syntax can often avoid importing older typing aliases.

---

# 91. CONTEXTLIB

Create context managers using a decorator:

```python
from contextlib import contextmanager

@contextmanager
def manager():

    print("Start")

    try:
        yield
    finally:
        print("End")
```

Use:

```python
with manager():
    print("Working")
```

---

# 92. TEMPORARY FILES

```python
import tempfile
```

Temporary file:

```python
with tempfile.NamedTemporaryFile() as file:
    file.write(b"Hello")
```

Temporary directory:

```python
with tempfile.TemporaryDirectory() as folder:
    print(folder)
```

Useful in:

```text
tests
file processing
temporary uploads
data transformation
```

---

# 93. SHUTIL

Useful for file operations.

```python
import shutil
```

Copy file:

```python
shutil.copy(
    "source.txt",
    "destination.txt"
)
```

Copy directory:

```python
shutil.copytree(
    "source",
    "destination"
)
```

Move:

```python
shutil.move(
    "old",
    "new"
)
```

Remove directory:

```python
shutil.rmtree("folder")
```

Be careful with `rmtree()` because it can recursively delete directories.

---

# 94. GLOB

```python
from pathlib import Path

files = Path(".").glob("*.py")

for file in files:
    print(file)
```

Recursive:

```python
Path(".").rglob("*.py")
```

---

# 95. CONFIGURATION FILES

Possible formats:

```text
.env
JSON
TOML
YAML
INI
```

For many Python projects, configuration values are separated from application code.

Example environment:

```text
DEBUG=True
DATABASE_URL=...
SECRET_KEY=...
```

---

# 96. COMMAND-LINE ARGUMENTS

Basic:

```python
import sys

print(sys.argv)
```

For proper CLI applications, use `argparse`.

```python
import argparse

parser = argparse.ArgumentParser()

parser.add_argument(
    "--name",
    required=True
)

args = parser.parse_args()

print(args.name)
```

Run:

```bash
python main.py --name Nzegge
```

---

# 97. PACKAGE STRUCTURE

A larger application might look like:

```text
myproject/
│
├── app/
│   ├── __init__.py
│   ├── models.py
│   ├── services.py
│   ├── utils.py
│   └── views.py
│
├── tests/
│   ├── __init__.py
│   └── test_app.py
│
├── .env
├── .gitignore
├── README.md
├── requirements.txt
└── pyproject.toml
```

---

# 98. PYPROJECT.TOML

Modern Python projects commonly use:

```text
pyproject.toml
```

It can contain project metadata and tool configuration.

Example:

```toml
[project]
name = "myproject"
version = "1.0.0"
description = "My Python project"
```

---

# 99. PYTHON PROJECT BEST PRACTICES

Use meaningful names:

```python
user_name = "Nzegge"
```

Avoid:

```python
x = "Nzegge"
```

unless the context is obvious.

Use functions for reusable logic.

Use classes when modeling objects/state makes sense.

Keep functions small.

Handle exceptions intentionally.

Use type hints for larger projects.

Use virtual environments.

Keep secrets out of Git.

Use `.gitignore`.

Write tests.

Use logging for applications.

Format and lint your code.

---

# 100. COMMON PYTHON STYLE

PEP 8 encourages readable code.

Good:

```python
def calculate_total(price, quantity):
    return price * quantity
```

Less readable:

```python
def ct(p,q):return p*q
```

Use four spaces for indentation.

Avoid unnecessary nested logic.

---

# 101. PYTHON GARBAGE COLLECTION

Python automatically manages objects that are no longer reachable.

Example:

```python
data = [1, 2, 3]

del data
```

`del` removes the name/reference; it does not necessarily mean immediate memory release.

---

# 102. DESCRIPTORS

Advanced Python objects can control attribute access using:

```python
__get__
__set__
__delete__
```

Example:

```python
class Descriptor:

    def __get__(self, instance, owner):
        return "value"
```

Descriptors are heavily used internally by features such as properties and frameworks.

---

# 103. METACLASSES

Classes are objects too.

The default metaclass is:

```python
type
```

Example:

```python
class User:
    pass

print(type(User))
```

Result:

```text
<class 'type'>
```

Metaclasses allow advanced customization of class creation.

Most applications do not need custom metaclasses.

---

# 104. DECORATOR + CLASS EXAMPLE

```python
from functools import wraps

def log_method(func):

    @wraps(func)
    def wrapper(*args, **kwargs):

        print(
            f"Calling {func.__name__}"
        )

        result = func(*args, **kwargs)

        print(
            f"Finished {func.__name__}"
        )

        return result

    return wrapper


class User:

    def __init__(self, name):
        self.name = name

    @log_method
    def greet(self):
        return f"Hello {self.name}"


user = User("Nzegge")

print(user.greet())
```

This combines:

```text
classes
objects
methods
decorators
functions
*args
**kwargs
```

---

# 105. PRACTICAL FILE PROCESSING PROJECT

```python
from pathlib import Path

folder = Path("documents")

folder.mkdir(
    exist_ok=True
)

for file in folder.glob("*.txt"):

    content = file.read_text(
        encoding="utf-8"
    )

    print(
        file.name,
        len(content)
    )
```

Concepts used:

```text
pathlib
directories
files
loops
strings
methods
encoding
```

---

# 106. PRACTICAL JSON PROJECT

```python
import json
from pathlib import Path

file = Path("users.json")

users = [
    {
        "name": "Nzegge",
        "age": 25
    },
    {
        "name": "John",
        "age": 30
    }
]

file.write_text(
    json.dumps(
        users,
        indent=4
    ),
    encoding="utf-8"
)

data = json.loads(
    file.read_text(
        encoding="utf-8"
    )
)

print(data)
```

---

# 107. PRACTICAL API PROJECT

```python
import requests

url = "https://example.com/api/users/"

try:

    response = requests.get(
        url,
        timeout=10
    )

    response.raise_for_status()

    data = response.json()

    print(data)

except requests.RequestException as error:

    print(
        f"Request failed: {error}"
    )
```

Important concepts:

```text
HTTP
GET
JSON
status codes
exceptions
timeouts
API
```

---

# 108. PRACTICAL OOP PROJECT

```python
class Product:

    def __init__(
        self,
        name: str,
        price: float,
        quantity: int
    ):
        self.name = name
        self.price = price
        self.quantity = quantity

    def total(self) -> float:
        return self.price * self.quantity

    def __str__(self):
        return (
            f"{self.name}: "
            f"{self.total():.2f}"
        )


product = Product(
    "Laptop",
    500,
    2
)

print(product)
```

---

# 109. COMMON BUILT-IN FUNCTIONS

Remember these:

```python
abs()
all()
any()
ascii()
bin()
bool()
breakpoint()
callable()
chr()
delattr()
dir()
divmod()
enumerate()
eval()
exec()
filter()
float()
format()
getattr()
globals()
hasattr()
hash()
help()
hex()
id()
input()
int()
isinstance()
issubclass()
iter()
len()
list()
locals()
map()
max()
memoryview()
min()
next()
object()
oct()
open()
ord()
pow()
print()
property()
range()
repr()
reversed()
round()
set()
setattr()
slice()
sorted()
staticmethod()
str()
sum()
super()
tuple()
type()
vars()
zip()
```

Some advanced/dangerous functions such as `eval()` and `exec()` should only be used with a very clear understanding of their security implications.

---

# 110. IMPORTANT STRING METHODS CHEAT SHEET

```python
str.lower()
str.upper()
str.capitalize()
str.title()
str.casefold()

str.strip()
str.lstrip()
str.rstrip()

str.split()
str.rsplit()
str.splitlines()

str.join()

str.replace()

str.find()
str.rfind()
str.index()
str.rindex()

str.startswith()
str.endswith()

str.count()

str.isalpha()
str.isdigit()
str.isdecimal()
str.isnumeric()
str.isalnum()
str.islower()
str.isupper()
str.isspace()
str.istitle()
str.isidentifier()

str.center()
str.ljust()
str.rjust()
str.zfill()

str.removeprefix()
str.removesuffix()

str.partition()
str.rpartition()
```

---

# 111. IMPORTANT LIST METHODS

```python
list.append()
list.extend()
list.insert()

list.remove()
list.pop()
list.clear()

list.index()
list.count()

list.sort()
list.reverse()

list.copy()
```

---

# 112. IMPORTANT DICTIONARY METHODS

```python
dict.keys()
dict.values()
dict.items()

dict.get()

dict.update()

dict.pop()
dict.popitem()

dict.setdefault()

dict.clear()
dict.copy()

dict.fromkeys()
```

---

# 113. IMPORTANT SET METHODS

```python
set.add()
set.remove()
set.discard()
set.pop()
set.clear()
set.copy()

set.update()

set.union()
set.intersection()
set.difference()
set.symmetric_difference()

set.issubset()
set.issuperset()
set.isdisjoint()
```

---

# 114. IMPORTANT FILE METHODS

```python
file.read()
file.readline()
file.readlines()

file.write()
file.writelines()

file.seek()
file.tell()

file.close()
```

Preferred pattern:

```python
with open(...) as file:
    ...
```

---

# 115. IMPORTANT DATETIME METHODS

```python
date.today()

datetime.now()
datetime.utcnow()   # avoid for new timezone-aware code

datetime.fromtimestamp()

datetime.strptime()
datetime.strftime()

date.replace()
datetime.replace()

date.weekday()
date.isoweekday()

date.isoformat()
datetime.isoformat()
```

---

# 116. IMPORTANT PATHLIB METHODS

```python
Path.cwd()

Path.exists()
Path.is_file()
Path.is_dir()

Path.mkdir()

Path.read_text()
Path.write_text()

Path.read_bytes()
Path.write_bytes()

Path.rename()
Path.replace()
Path.unlink()

Path.iterdir()

Path.glob()
Path.rglob()

Path.name
Path.stem
Path.suffix
Path.parent
Path.parts
```

---

# 117. COMMON HTTP STATUS CODES

```text
200 OK
201 Created
202 Accepted
204 No Content

301 Moved Permanently
302 Found
304 Not Modified

400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
405 Method Not Allowed
409 Conflict
422 Unprocessable Content
429 Too Many Requests

500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
504 Gateway Timeout
```

---

# 118. PYTHON WEB DEVELOPMENT

Popular frameworks:

```text
Django
Django REST Framework
Flask
FastAPI
```

Typical backend architecture:

```text
Client
   ↓
HTTP Request
   ↓
Router / URL
   ↓
View / Controller
   ↓
Service / Business Logic
   ↓
Model / Database
   ↓
Response
   ↓
Client
```

---

# 119. PYTHON + DJANGO CONCEPTS

When moving from Python into Django, learn:

```text
Models
Views
URLs
Templates
Forms
Middleware
Authentication
Permissions
Sessions
ORM
Migrations
Admin
Signals
Caching
Celery
REST APIs
Serializers
Authentication
JWT
Testing
```

---

# 120. PYTHON + DJANGO REST FRAMEWORK

Important concepts:

```text
APIView
GenericAPIView
ViewSet
ModelViewSet
Serializers
ModelSerializer
Routers
Permissions
Authentication
Throttling
Pagination
Filtering
Parsers
Renderers
Validators
Exceptions
```

Typical request:

```text
React
  ↓
HTTP request
  ↓
Django URL
  ↓
DRF View
  ↓
Serializer
  ↓
Model
  ↓
Database
  ↓
Serializer
  ↓
JSON response
  ↓
React
```

---

# 121. DEBUGGING PYTHON

Read the traceback from the bottom upward.

Example:

```text
Traceback...
File "views.py", line 20
...
ValueError: invalid literal
```

The final line usually contains the exception type and main message.

Useful:

```python
print(value)
print(type(value))
print(repr(value))
```

Use debugger:

```python
breakpoint()
```

Then inspect variables.

---

# 122. COMMON ERRORS

## NameError

```python
print(username)
```

when `username` does not exist.

---

## TypeError

Wrong operation/type:

```python
"10" + 5
```

---

## ValueError

Correct type but invalid value:

```python
int("hello")
```

---

## IndexError

Invalid list index:

```python
items[100]
```

---

## KeyError

Missing dictionary key:

```python
user["email"]
```

---

## AttributeError

Object does not have attribute/method:

```python
value.some_method()
```

---

## FileNotFoundError

```python
open("missing.txt")
```

---

## ZeroDivisionError

```python
10 / 0
```

---

## ImportError

Cannot import requested object.

---

## ModuleNotFoundError

Python cannot find the module.

---

# 123. ERROR-HANDLING PATTERN

Good:

```python
try:

    result = process_data()

except ValueError as error:

    print(
        f"Invalid data: {error}"
    )

except FileNotFoundError:

    print("File does not exist")

else:

    print("Success")

finally:

    print("Finished")
```

Avoid:

```python
try:
    ...
except:
    pass
```

because it hides errors.

---

# 124. CLEAN CODE PRINCIPLES

Follow:

```text
DRY
KISS
Single Responsibility
Clear naming
Small functions
Separation of concerns
Explicit error handling
Tests
Documentation
```

DRY:

```text
Don't Repeat Yourself
```

KISS:

```text
Keep It Simple
```

---

# 125. IMPORTANT PYTHON PROJECT WORKFLOW

Create project:

```bash
mkdir myproject
cd myproject
```

Create environment:

```bash
python -m venv venv
```

Activate:

```bash
venv\Scripts\activate
```

Upgrade pip:

```bash
python -m pip install --upgrade pip
```

Install packages:

```bash
pip install requests
```

Save dependencies:

```bash
pip freeze > requirements.txt
```

Create Git repository:

```bash
git init
```

Create `.gitignore`:

```text
venv/
__pycache__/
*.pyc
.env
```

---

# 126. PYTHON FILE NAMING

Recommended:

```text
snake_case.py
```

Examples:

```text
user_service.py
product_model.py
database_utils.py
```

Classes:

```python
class UserService:
    pass
```

Functions:

```python
def calculate_total():
    pass
```

Constants:

```python
MAX_RETRIES = 3
```

---

# 127. WHEN TO USE EACH DATA TYPE

## List

Use when:

```text
ordered collection
duplicates allowed
items may change
```

Example:

```python
products = ["phone", "laptop"]
```

## Tuple

Use when:

```text
ordered
immutable
fixed structure
```

Example:

```python
coordinates = (10, 20)
```

## Set

Use when:

```text
unique values
fast membership testing
set operations
```

Example:

```python
permissions = {"read", "write"}
```

## Dictionary

Use when:

```text
key → value
lookup
structured data
```

Example:

```python
user = {
    "name": "Nzegge",
    "age": 25
}
```

---

# 128. MUTABLE VS IMMUTABLE

Mutable:

```text
list
dict
set
bytearray
```

Immutable:

```text
int
float
bool
str
tuple
frozenset
bytes
```

Example:

```python
numbers = [1, 2, 3]

numbers.append(4)
```

The list itself changed.

---

# 129. SHALLOW VS DEEP COPY

Example:

```python
import copy

original = [
    [1, 2],
    [3, 4]
]

shallow = copy.copy(original)
deep = copy.deepcopy(original)
```

Use deep copy when independent nested objects are required.

---

# 130. PYTHON PACKAGE ECOSYSTEM

Common packages you may encounter:

```text
requests       → HTTP requests
Django         → web framework
djangorestframework → REST APIs
FastAPI        → API framework
Flask          → web framework
SQLAlchemy     → database toolkit/ORM
pytest         → testing
Pydantic       → validation/data models
Celery         → task queue
redis          → Redis client
Pillow         → image processing
pandas         → data analysis
numpy          → numerical computing
```

Install:

```bash
pip install package-name
```

---

# 131. TESTING LEVELS

Unit test:

```text
test one function/class
```

Integration test:

```text
test multiple components together
```

API test:

```text
test HTTP endpoints
```

End-to-end test:

```text
test complete application workflow
```

---

# 132. UNIT TEST EXAMPLE

```python
def multiply(a, b):
    return a * b
```

Test:

```python
import unittest


class TestMultiply(unittest.TestCase):

    def test_multiply(self):

        result = multiply(3, 4)

        self.assertEqual(
            result,
            12
        )


if __name__ == "__main__":
    unittest.main()
```

---

# 133. MOCK EXAMPLE

```python
from unittest.mock import Mock

payment_service = Mock()

payment_service.charge.return_value = True

result = payment_service.charge(100)

assert result is True

payment_service.charge.assert_called_once_with(100)
```

---

# 134. DATABASE TRANSACTION CONCEPT

A transaction groups database operations so they succeed or fail together.

Concept:

```text
BEGIN
   operation 1
   operation 2
   operation 3
COMMIT
```

If something fails:

```text
ROLLBACK
```

This concept is especially important when building e-commerce applications involving:

```text
orders
payments
inventory
users
transactions
```

---

# 135. CACHING CONCEPT

Cache stores frequently used data so it can be retrieved faster.

Example:

```text
Client
  ↓
Cache
  ↓
If found → return cached result

If missing
  ↓
Database
  ↓
Store result in cache
  ↓
Return result
```

Common technologies:

```text
Redis
Memcached
```

---

# 136. BACKGROUND TASKS

Some operations should not block an HTTP request:

```text
send email
generate report
resize image
process large file
send notification
generate invoice
```

Typical architecture:

```text
Web request
    ↓
Queue task
    ↓
Return response
    ↓
Worker processes task
```

Common Python technology:

```text
Celery
```

---

# 137. PYTHON ASYNCHRONOUS ARCHITECTURE

For async applications:

```text
Client
   ↓
Async server
   ↓
async function
   ↓
await I/O
   ↓
other work can run
```

Useful for:

```text
web APIs
network calls
web sockets
high-concurrency I/O
```

---

# 138. ADVANCED LEARNING TOPICS

After mastering the above, study:

```text
Descriptors
Metaclasses
Protocols
Generics
Advanced decorators
Context managers
Async programming
Event loops
Thread synchronization
Multiprocessing
Memory optimization
Profiling
Packaging
Dependency management
Design patterns
Clean architecture
Testing architecture
Security
Database optimization
API architecture
Distributed systems
```

---

# 139. DESIGN PATTERNS TO LEARN

Useful patterns:

```text
Factory
Strategy
Observer
Adapter
Decorator
Repository
Service Layer
Singleton
Dependency Injection
```

Do not use patterns simply for the sake of using them. Choose a pattern when it solves a real design problem.

---

# 140. PYTHON LEARNING ROADMAP

## LEVEL 1 — BEGINNER

Learn:

```text
variables
strings
numbers
booleans
None
lists
tuples
sets
dictionaries
operators
if/elif/else
for
while
functions
input()
print()
```

Projects:

```text
calculator
number guessing game
todo list
simple quiz
unit converter
```

---

## LEVEL 2 — INTERMEDIATE

Learn:

```text
exceptions
files
JSON
CSV
modules
packages
virtual environments
pip
OOP
inheritance
properties
decorators
generators
iterators
datetime
pathlib
os
regex
```

Projects:

```text
contact manager
file organizer
expense tracker
JSON database
CLI application
```

---

## LEVEL 3 — ADVANCED

Learn:

```text
typing
dataclasses
asyncio
threading
multiprocessing
testing
mocking
logging
database programming
APIs
authentication concepts
caching
background tasks
packaging
architecture
security
```

Projects:

```text
REST API
authentication system
e-commerce backend
file upload API
task queue
web scraper
database application
```

---

# 141. FINAL PYTHON CHEAT SHEET

```python
# VARIABLES
name = "Nzegge"
age = 25

# TYPES
type(name)
isinstance(age, int)

# STRING
name.lower()
name.upper()
name.strip()
name.split()
name.replace("a", "b")

# LIST
items.append(x)
items.extend(values)
items.insert(index, value)
items.remove(value)
items.pop()
items.sort()
items.reverse()

# DICT
data.keys()
data.values()
data.items()
data.get("key")
data.update({...})
data.pop("key")

# SET
items.add(x)
items.remove(x)
items.union(other)
items.intersection(other)
items.difference(other)

# CONDITIONS
if condition:
    pass
elif condition:
    pass
else:
    pass

# LOOP
for item in items:
    pass

while condition:
    pass

# FUNCTION
def function(value):
    return value

# *ARGS
def function(*args):
    pass

# **KWARGS
def function(**kwargs):
    pass

# EXCEPTION
try:
    pass
except Exception as error:
    pass
finally:
    pass

# FILE
with open("file.txt", "r", encoding="utf-8") as file:
    content = file.read()

# PATH
from pathlib import Path

path = Path("file.txt")

# DATE
from datetime import date, datetime, timedelta

today = date.today()
now = datetime.now()

# JSON
import json

json.dumps(data)
json.loads(text)

# OS
import os

os.getcwd()
os.listdir()
os.getenv("NAME")

# REGEX
import re

re.search(pattern, text)
re.findall(pattern, text)
re.sub(pattern, replacement, text)

# OOP
class User:

    def __init__(self, name):
        self.name = name

    def greet(self):
        return f"Hello {self.name}"

# DECORATOR
@decorator
def function():
    pass

# GENERATOR
def numbers():
    yield 1
    yield 2

# TEST
import unittest

# HTTP
import requests

response = requests.get(
    url,
    timeout=10
)

# LOGGING
import logging

logger = logging.getLogger(__name__)
logger.info("Message")

# ASYNC
async def task():
    await something()

# COMMAND LINE
python main.py

# VIRTUAL ENVIRONMENT
python -m venv venv

# WINDOWS
venv\Scripts\activate

# INSTALL PACKAGE
pip install package

# SAVE PACKAGES
pip freeze > requirements.txt

# INSTALL REQUIREMENTS
pip install -r requirements.txt
```

---

# 142. THE BIG PICTURE

Python can be understood as several layers:

```text
                    PYTHON
                       │
        ┌──────────────┴──────────────┐
        │                             │
      BASICS                       ADVANCED
        │                             │
 ┌──────┴──────┐             ┌────────┴────────┐
 │             │             │                 │
Variables    Data Types     OOP              Async
Operators    Functions      Decorators       Threads
Conditions   Loops          Generators       Processes
             │              Iterators        Queues
             │
      ┌──────┴────────┐
      │               │
    FILES            DATA
      │               │
    OS               JSON
    pathlib          CSV
    shutil           Databases
      │
      └──────────┬───────────
                 │
              WEB/API
                 │
          ┌──────┴──────┐
          │             │
        Django        FastAPI
          │
        DRF
          │
       REST API
          │
       Database
          │
       Frontend
```

The important progression is:

```text
Python Syntax
      ↓
Data Types
      ↓
Functions
      ↓
Data Structures
      ↓
Files + OS
      ↓
Exceptions
      ↓
Modules + Packages
      ↓
OOP
      ↓
Iterators + Generators
      ↓
Decorators
      ↓
Testing
      ↓
Databases
      ↓
HTTP / APIs
      ↓
Async / Concurrency
      ↓
Frameworks
      ↓
Architecture
      ↓
Production Applications
```

---

# 143. MOST IMPORTANT RULE

Do not try to memorize every method.

Instead, learn how to discover them:

```python
dir(object)
```

Then:

```python
help(object.method)
```

And inspect:

```python
type(object)
```

Example:

```python
name = "Python"

print(type(name))
print(dir(name))
help(name.split)
```

This is one of the most valuable Python skills because Python has a very large standard library and ecosystem.

---

# 144. FINAL PRACTICE STRATEGY

For every Python concept, use this sequence:

```text
1. Understand the concept
        ↓
2. Learn the syntax
        ↓
3. Learn the methods
        ↓
4. Write a tiny example
        ↓
5. Make a mistake intentionally
        ↓
6. Read the error
        ↓
7. Fix it
        ↓
8. Build a small project
        ↓
9. Use the concept in a larger project
```

For example:

```text
LIST
 ↓
learn append()
 ↓
learn remove()
 ↓
learn sort()
 ↓
build Todo List
 ↓
store Todo List in JSON
 ↓
create CLI Todo application
 ↓
create REST API for Todo List
```

This turns Python knowledge into practical programming ability.
