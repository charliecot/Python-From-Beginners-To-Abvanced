# PYTHON REGEX & VALIDATION — COMPLETE REFERENCE

A practical reference from beginner to advanced.

This document covers:

* Python `re` module
* Regex syntax
* Regex character classes
* Quantifiers
* Anchors
* Groups
* Lookaheads/lookbehinds
* Greedy/lazy matching
* Regex flags
* `search()`, `match()`, `fullmatch()`
* `findall()`, `finditer()`
* `sub()`, `split()`
* `compile()`
* `any()`
* `all()`
* `in`
* `not in`
* `startswith()`
* `endswith()`
* `isdigit()`
* `isdecimal()`
* `isnumeric()`
* `isalpha()`
* `isalnum()`
* `isspace()`
* `islower()`
* `isupper()`
* `istitle()`
* `isidentifier()`
* `isinstance()`
* `len()`
* `strip()`
* `lower()`
* `casefold()`
* `try/except`
* `assert`
* comprehensions
* `enumerate()`
* `zip()`
* `filter()`
* dictionary validation
* file validation
* URL validation
* IP validation
* date validation
* integer/float validation
* password validation
* username validation
* form validation
* validation design principles

============================================================

1. WHAT IS VALIDATION?
   ============================================================

Validation means checking whether data follows rules before using it.

Example:

```
username = "nzegge123"
```

You may want to check:

```
- Is it a string?
- Is it empty?
- Is it between 3 and 20 characters?
- Does it contain spaces?
- Does it contain only allowed characters?
- Does it start with a letter?
- Is the username already forbidden?
```

Python provides many tools for this.

Think about validation in layers:

```
INPUT
  |
  v
TYPE
  |
  v
EMPTY?
  |
  v
LENGTH
  |
  v
CHARACTERS
  |
  v
FORMAT
  |
  v
BUSINESS RULES
  |
  v
VALID DATA
```

Common tools:

```
isinstance()
type()
bool()
len()
in
not in
startswith()
endswith()
strip()
lower()
casefold()
isdigit()
isdecimal()
isnumeric()
isalpha()
isalnum()
isspace()
islower()
isupper()
istitle()
isidentifier()
any()
all()
try/except
re
datetime
ipaddress
urllib.parse
pathlib
```

Python's regular expression module is called `re`.

```
import re
```

Example:

```
import re

text = "My age is 25"

result = re.search(r"\d+", text)

print(result)
```

Regex means Regular Expression.

A regex is a pattern used to find, extract, replace, or validate text.

Example:

```
r"\d+"
```

Means:

```
\d  = digit
+   = one or more
```

Therefore:

```
"abc123"
```

contains:

```
123
```

Prefer raw strings when writing regex:

```
r"\d+"
```

instead of:

```
"\d+"
```

The `r` tells Python to treat backslashes more literally.

Example:

```
pattern = r"\d+\.\d+"
```

This represents:

```
digits
followed by .
followed by digits
```

Example:

```
12.50
```

The main functions are:

```
re.search()
re.match()
re.fullmatch()
re.findall()
re.finditer()
re.split()
re.sub()
re.subn()
re.compile()
re.escape()
```

`search()` looks for a pattern anywhere in the string.

```
import re

text = "My age is 25"

result = re.search(r"\d+", text)

if result:
    print("Number found:", result.group())
```

Example:

```
re.search(r"\d", "abc123")
```

Matches because a digit exists somewhere.

IMPORTANT:

```
search() -> anywhere
```

`match()` checks from the beginning of the string.

```
re.match(r"Hello", "Hello World")
```

Matches.

But:

```
re.match(r"Hello", "World Hello")
```

Does not match.

Remember:

```
search() -> anywhere
match()  -> beginning
```

`fullmatch()` requires the entire string to match.

This is extremely useful for validation.

```
import re

username = "nzegge123"

if re.fullmatch(r"[A-Za-z0-9]+", username):
    print("Valid")
else:
    print("Invalid")
```

The entire string must contain only letters and numbers.

For validation, remember:

```
re.fullmatch()
```

Find all matching pieces.

```
import re

text = "I have 10 apples and 20 oranges"

numbers = re.findall(r"\d+", text)

print(numbers)
```

Output:

```
['10', '20']
```

`findall()` returns a list.

`finditer()` returns match objects.

```
text = "I have 10 apples and 20 oranges"

for match in re.finditer(r"\d+", text):
    print(match.group())
    print(match.start())
    print(match.end())
```

Useful when you need:

```
matched text
starting position
ending position
groups
```

Example:

```
match = re.search(r"\d+", "Age: 25")

print(match.group())
print(match.start())
print(match.end())
```

Methods:

```
group()
start()
end()
span()
```

`group()`:

```
returns matched text
```

`start()`:

```
starting position
```

`end()`:

```
position immediately after the match
```

`span()`:

```
returns:

(start, end)
```

```
\d
\D
\w
\W
\s
\S
.
```

Meaning:

```
\d  -> digit
\D  -> not a digit

\w  -> word character
\W  -> non-word character

\s  -> whitespace
\S  -> non-whitespace

.   -> almost any character
```

```
re.findall(r"\d", "abc123")
```

Result:

```
['1', '2', '3']
```

One or more:

```
re.findall(r"\d+", "abc123 xyz45")
```

Result:

```
['123', '45']
```

```
re.findall(r"\D+", "123abc456xyz")
```

Result:

```
['abc', 'xyz']
```

```
\w
```

Generally matches:

```
letters
digits
underscore
```

Example:

```
re.findall(r"\w+", "hello_world 123")
```

```
re.findall(r"\W+", "hello!!! world")
```

Matches characters such as:

```
!
spaces
punctuation
```

Matches:

```
spaces
tabs
newlines
```

Example:

```
re.findall(r"\s+", "hello   world")
```

```
re.findall(r"\S+", "hello world")
```

Result:

```
['hello', 'world']
```

`.` means almost any character.

```
re.findall(r"h.t", "hat hit hot")
```

Matches:

```
hat
hit
hot
```

Character sets specify allowed characters.

```
[abc]
```

means:

```
a OR b OR c
```

Example:

```
re.findall(r"[aeiou]", "hello")
```

Result:

```
['e', 'o']
```

Instead of:

```
[abcdefghijklmnopqrstuvwxyz]
```

use:

```
[a-z]
```

Uppercase:

```
[A-Z]
```

Digits:

```
[0-9]
```

Letters:

```
[A-Za-z]
```

Letters and numbers:

```
[A-Za-z0-9]
```

Put `^` immediately after `[`.

```
[^0-9]
```

Means:

```
anything except a digit
```

Example:

```
re.findall(r"[^0-9]+", "123abc456")
```

Result:

```
['abc']
```

Quantifiers specify how many times something can occur.

```
*       zero or more
+       one or more
?       zero or one
{n}     exactly n
{n,}    n or more
{n,m}   between n and m
```

Zero or more.

```
r"a*"
```

Can match:

```
""
"a"
"aa"
"aaa"
```

One or more.

```
r"a+"
```

Matches:

```
a
aa
aaa
```

Does not match an empty string.

Zero or one.

Example:

```
r"colou?r"
```

Matches:

```
color
colour
```

Exactly 3 digits:

```
r"\d{3}"
```

Examples:

```
123      VALID
1234     INVALID
12       INVALID
```

At least 3:

```
r"\d{3,}"
```

Matches:

```
123
1234
12345
```

3 to 5:

```
r"\d{3,5}"
```

Examples:

```
12       INVALID
123      VALID
1234     VALID
12345    VALID
123456   INVALID
```

Important anchors:

```
^
$
\b
\B
```

Beginning of string.

```
r"^Hello"
```

Matches:

```
Hello World
```

Does not match:

```
Say Hello
```

End of string.

```
r"world$"
```

Matches:

```
Hello world
```

Does not match:

```
world today
```

Example:

```
r"^[A-Za-z]+$"
```

Means:

```
beginning
letters
end
```

Therefore:

```
hello          VALID
Hello          VALID
hello123       INVALID
hello world    INVALID
```

For validation, however, this is often cleaner:

```
re.fullmatch(r"[A-Za-z]+", value)
```

A word boundary exists around word/non-word transitions.

Example:

```
text = "cat category"

re.findall(r"\bcat\b", text)
```

Result:

```
['cat']
```

It does not match `cat` inside `category`.

`\B` means:

```
NOT a word boundary
```

It is mainly useful in advanced patterns.

`|` means OR.

```
r"cat|dog"
```

Matches:

```
cat
```

or:

```
dog
```

Example:

```
pattern = r"python|javascript|django"

if re.search(pattern, text):
    print("Technology found")
```

Parentheses group patterns.

```
r"(cat|dog)"
```

You can also capture information:

```
text = "John is 25"

match = re.search(r"(\w+) is (\d+)", text)

print(match.group(1))
print(match.group(2))
```

Output:

```
John
25
```

`group(0)` returns the entire match.

```
match.group(0)
```

While:

```
group(1)
group(2)
```

refer to captured groups.

Instead of remembering group numbers:

```
(?P<name>pattern)
```

Example:

```
pattern = r"(?P<name>\w+) is (?P<age>\d+)"

match = re.search(pattern, "John is 25")

print(match.group("name"))
print(match.group("age"))
```

Output:

```
John
25
```

Named groups are excellent for complex regex.

Use:

```
(?:...)
```

Example:

```
r"(?:cat|dog)"
```

This groups alternatives without creating a numbered capture group.

Positive lookahead:

```
(?=...)
```

It means:

```
The following pattern must exist,
but don't consume it.
```

Example:

```
r"(?=.*\d)"
```

Means:

```
There must be a digit somewhere in the string.
```

```
(?!...)
```

Means:

```
The pattern must NOT occur here.
```

Example:

```
r"^(?!.*password).+$"
```

This rejects strings containing `password` when matching is case-sensitive.

For case-insensitive checking:

```
re.fullmatch(
    r"^(?!.*password).+$",
    value,
    re.IGNORECASE
)
```

Positive lookbehind:

```
(?<=...)
```

Example:

```
text = "Price: $100"

match = re.search(r"(?<=\$)\d+", text)

print(match.group())
```

Output:

```
100
```

The number must be immediately after `$`.

```
(?<!...)
```

Means:

```
The previous characters must NOT match.
```

Example:

```
r"(?<!\$)\d+"
```

Can find numbers that are not immediately preceded by `$`.

Regex quantifiers are normally greedy.

Example:

```
text = "<h1>Hello</h1><h1>World</h1>"

re.findall(r"<h1>.*</h1>", text)
```

`.*` may consume as much as possible.

Add `?`.

```
*?
+?
??
{n,m}?
```

Example:

```
re.findall(r"<h1>.*?</h1>", text)
```

This tries to consume as little as possible.

Common flags:

```
re.IGNORECASE
re.I

re.MULTILINE
re.M

re.DOTALL
re.S

re.VERBOSE
re.X
```

Case-insensitive matching.

```
re.search(
    r"python",
    "PYTHON",
    re.IGNORECASE
)
```

Also:

```
re.I
```

Allows `^` and `$` to work with individual lines.

```
text = """hello
world
hello"""

matches = re.findall(
    r"^hello$",
    text,
    re.MULTILINE
)
```

Normally `.` does not match newline.

`re.DOTALL` makes it match newlines.

```
re.search(
    r"hello.*world",
    text,
    re.DOTALL
)
```

Useful for large regex.

Example:

```
pattern = re.compile(r"""
    ^
    (?=.*[A-Z])          # uppercase
    (?=.*[a-z])          # lowercase
    (?=.*\d)             # digit
    (?=.*[^A-Za-z0-9])   # special character
    .{8,}                # minimum 8 chars
    $
""", re.VERBOSE)
```

This makes complex regex much easier to read.

Compile a regex for reuse.

```
pattern = re.compile(r"\d+")

print(pattern.findall("123 abc 456"))

print(pattern.findall("10 xyz 99"))
```

You can use:

```
pattern.search()
pattern.match()
pattern.fullmatch()
pattern.findall()
pattern.finditer()
pattern.sub()
```

Replace matches.

```
text = "hello     world"

result = re.sub(r"\s+", " ", text)

print(result)
```

Output:

```
hello world
```

```
phone = "+237 690-123-456"

clean = re.sub(r"\D", "", phone)

print(clean)
```

Output:

```
237690123456
```

Split using a regex.

```
text = "apple,banana;orange|mango"

items = re.split(r"[,;|]", text)

print(items)
```

Output:

```
['apple', 'banana', 'orange', 'mango']
```

Like `sub()`, but also tells you how many replacements happened.

```
result, count = re.subn(r"\d+", "NUMBER", "10 cats 20 dogs")

print(result)
print(count)
```

Treat user input as literal text instead of regex syntax.

```
user_input = "hello.world"

pattern = re.escape(user_input)
```

The `.` will be escaped.

Requirements:

```
- 3 to 20 characters
- letters
- numbers
- underscore
- starts with letter
```

Regex:

```
pattern = r"[A-Za-z][A-Za-z0-9_]{2,19}"

username = "nzegge_123"

if re.fullmatch(pattern, username):
    print("Valid username")
else:
    print("Invalid username")
```

Basic practical pattern:

```
pattern = (
    r"[A-Za-z0-9._%+-]+"
    r"@"
    r"[A-Za-z0-9.-]+"
    r"\."
    r"[A-Za-z]{2,}"
)

email = "nzegge@example.com"

if re.fullmatch(pattern, email):
    print("Valid email")
```

Do not try to implement every possible email-standard edge case using one giant regex.

For production applications, consider a dedicated validation library/framework validator.

Example Cameroon number:

```
phone = "+237690123456"

pattern = r"\+237\d{9}"

if re.fullmatch(pattern, phone):
    print("Valid")
```

Allow optional `+237`:

```
pattern = r"(?:\+237|237)?\d{9}"
```

Requirements:

```
- minimum 8 characters
- uppercase
- lowercase
- number
- special character
```

Pattern:

```
pattern = (
    r"(?=.*[A-Z])"
    r"(?=.*[a-z])"
    r"(?=.*\d)"
    r"(?=.*[^A-Za-z0-9])"
    r".{8,}"
)

if re.fullmatch(pattern, password):
    print("Strong password")
```

Often easier to understand:

```
password = "Hello123!"

valid = (
    len(password) >= 8
    and any(c.isupper() for c in password)
    and any(c.islower() for c in password)
    and any(c.isdigit() for c in password)
    and any(not c.isalnum() for c in password)
)

print(valid)
```

This is a very important technique.

`in` checks membership.

String:

```
username = "nzegge"

if "@" in username:
    print("Contains @")
```

List:

```
allowed_roles = ["admin", "staff", "customer"]

role = "admin"

if role in allowed_roles:
    print("Allowed")
```

Dictionary:

```
user = {
    "name": "Nzegge",
    "age": 25
}

if "name" in user:
    print("Name exists")
```

Checks that something does not exist.

```
if "@" not in username:
    print("No @ found")
```

Example:

```
blocked = ["admin", "root", "superuser"]

username = "nzegge"

if username not in blocked:
    print("Username allowed")
```

Check beginning of string.

```
filename = "profile.jpg"

if filename.startswith("profile"):
    print("Profile file")
```

Multiple prefixes:

```
if filename.startswith(
    ("profile", "avatar", "user")
):
    print("Valid prefix")
```

Excellent for extensions.

```
filename = "photo.jpg"

if filename.endswith(".jpg"):
    print("JPEG")
```

Multiple extensions:

```
allowed = (
    ".jpg",
    ".jpeg",
    ".png",
    ".webp"
)

if filename.lower().endswith(allowed):
    print("Image allowed")
```

Remove whitespace around a string.

```
username = "   nzegge   "

username = username.strip()
```

Result:

```
"nzegge"
```

Also:

```
lstrip()
rstrip()
```

Convert to lowercase.

```
email = "NZEgge@GMAIL.COM"

email = email.lower()
```

Useful for normalization before comparison.

Convert to uppercase.

```
code = "abc123"

code = code.upper()
```

Result:

```
ABC123
```

Strong case-insensitive normalization.

```
username.casefold()
```

Useful when comparing text without caring about case.

Example:

```
if username.casefold() == "admin":
    print("Admin")
```

Checks whether all characters are digits.

```
"12345".isdigit()
```

Result:

```
True


"123abc".isdigit()
```

Result:

```
False
```

IMPORTANT:

```
"-123".isdigit()
```

is:

```
False
```

So `isdigit()` is not always enough for signed numbers.

Checks whether all characters are decimal characters.

```
"123".isdecimal()
```

Result:

```
True
```

Checks whether all characters are numeric.

```
"123".isnumeric()
```

Result:

```
True
```

`isnumeric()` covers a broader range of numeric Unicode characters than `isdigit()`.

Checks whether all characters are alphabetic.

```
"hello".isalpha()

True


"hello123".isalpha()

False
```

Letters and numbers.

```
"hello123".isalnum()

True
```

But:

```
"hello_123".isalnum()

False
```

because `_` isn't alphanumeric.

Checks whether characters are whitespace.

```
"   ".isspace()

True
```

Useful for detecting strings containing only spaces/tabs/newlines.

```
"hello".islower()

True
```

```
"HELLO".isupper()

True
```

```
"Hello World".istitle()

True
```

Checks whether text is a valid Python identifier.

```
"my_variable".isidentifier()

True


"123name".isidentifier()

False
```

Useful when validating Python-style names.

Check length.

```
username = "nzegge"

if 3 <= len(username) <= 20:
    print("Valid length")
```

This is often better than using regex for simple length rules.

Check data type.

```
value = 123

if isinstance(value, int):
    print("Integer")
```

Multiple accepted types:

```
if isinstance(value, (int, float)):
    print("Number")
```

Generally prefer `isinstance()` over:

```
type(value) is int
```

because `isinstance()` handles inheritance more naturally.

You can inspect a value's exact type.

```
type(value)
```

Example:

```
if type(value) is int:
    print("Integer")
```

For general type validation, prefer:

```
isinstance()
```

Convert to Boolean.

```
bool("hello")

True


bool("")

False
```

Python considers these generally false:

```
False
None
0
0.0
""
[]
{}
set()
```

Instead of:

```
if value == "":
```

Prefer:

```
if not value:
    print("Empty")
```

For strings where spaces should count as empty:

```
if not value.strip():
    print("Empty")
```

Use:

```
if value is None:
    print("No value")
```

And:

```
if value is not None:
    print("Value exists")
```

Prefer this over:

```
if value == None
```

`any()` returns True if at least ONE item is truthy.

Example:

```
numbers = [1, 3, 5, 8]

if any(n % 2 == 0 for n in numbers):
    print("At least one even number")
```

Because:

```
1 -> False
3 -> False
5 -> False
8 -> True
```

Therefore:

```
any() -> at least one
```

Contains a digit:

```
any(c.isdigit() for c in password)
```

Contains uppercase:

```
any(c.isupper() for c in password)
```

Contains lowercase:

```
any(c.islower() for c in password)
```

Contains whitespace:

```
any(c.isspace() for c in password)
```

Contains special character:

```
any(not c.isalnum() for c in password)
```

Check whether a string contains one of several characters.

```
password = "Hello@123"

if any(c in "@#$%" for c in password):
    print("Contains special character")
```

Another example:

```
if any(word in text for word in ["python", "django", "react"]):
    print("Technology mentioned")
```

`all()` returns True only if EVERY item is truthy.

```
numbers = [2, 4, 6, 8]

if all(n % 2 == 0 for n in numbers):
    print("All are even")
```

Therefore:

```
any() -> at least one
all() -> every one
```

Username may contain:

```
letters
numbers
underscore
```

Use:

```
username = "nzegge_123"

valid = all(
    c.isalnum() or c == "_"
    for c in username
)

print(valid)
```

```
allowed = (
    "abcdefghijklmnopqrstuvwxyz"
    "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    "0123456789_"
)

valid = all(
    c in allowed
    for c in username
)
```

Remember:

```
any() -> ONE OR MORE
all() -> EVERY ITEM
```

Example:

```
values = [2, 4, 7]

any(v % 2 == 0 for v in values)

True
```

But:

```
all(v % 2 == 0 for v in values)

False
```

Very powerful combination:

```
username = "nzegge_123"

valid = (
    3 <= len(username) <= 20
    and username[0].isalpha()
    and all(
        c.isalnum() or c == "_"
        for c in username
    )
)
```

```
username = "nzegge"

valid = (
    3 <= len(username) <= 20
    and " " not in username
    and "@" not in username
)
```

Use `try/except` when Python itself can tell you whether a value can be converted.

Integer:

```
value = "123"

try:
    number = int(value)
except ValueError:
    print("Invalid integer")
else:
    print("Valid integer:", number)
```

Simple positive integer:

```
value = "123"

if value.isdigit():
    number = int(value)
```

But if negative numbers are allowed:

```
value = "-123"

try:
    number = int(value)
except ValueError:
    print("Invalid integer")
```

This is usually better than regex.

```
value = "12.50"

try:
    number = float(value)
except ValueError:
    print("Invalid number")
else:
    print(number)
```

This handles values such as:

```
12
12.5
-12.5
1e5
```

`assert` checks assumptions.

```
age = 20

assert age >= 18
```

With message:

```
assert age >= 18, "User must be an adult"
```

Important:

Do not use `assert` as your primary user-input validation mechanism.

Assertions can be disabled when Python is run in optimized mode.

Use normal validation for application input.

Useful when validating a sequence and needing its position.

```
items = ["apple", "", "orange"]

for index, item in enumerate(items):

    if not item:
        print(f"Item {index} is empty")
```

Combine related sequences.

```
names = ["John", "Mary"]
ages = [20, 25]

for name, age in zip(names, ages):
    print(name, age)
```

Useful when validating corresponding values.

Find invalid values.

```
users = [
    "john",
    "mary",
    "",
    "bob"
]

invalid = [
    user
    for user in users
    if not user
]

print(invalid)
```

Result:

```
['']
```

Example:

```
user = {
    "name": "Nzegge",
    "email": "nzegge@example.com"
}
```

Check key:

```
if "email" in user:
    print("Email exists")
```

Get safely:

```
email = user.get("email")
```

Default:

```
email = user.get(
    "email",
    "No email"
)
```

Check values:

```
if "Nzegge" in user.values():
    print("Name exists")
```

Checks whether an object can be called like a function.

```
def hello():
    pass

print(callable(hello))
```

Output:

```
True
```

Useful when validating callbacks/functions.

Example:

```
numbers = [1, 2, 3, 4, 5]

even = list(
    filter(
        lambda x: x % 2 == 0,
        numbers
    )
)
```

Often a comprehension is easier:

```
even = [
    x
    for x in numbers
    if x % 2 == 0
]
```

```
filename = "profile.png"

allowed = (
    ".jpg",
    ".jpeg",
    ".png",
    ".webp"
)

if filename.lower().endswith(allowed):
    print("Allowed image")
```

IMPORTANT:

An extension alone is not enough for secure file-upload validation.

A user can rename:

```
malicious.exe
```

to:

```
photo.jpg
```

For real applications also validate:

```
file size
actual file type/content
upload location
filename
permissions
server-side handling
```

Simple check:

```
url = "https://example.com"

valid = (
    url.startswith("http://")
    or url.startswith("https://")
)
```

For proper URL parsing, use:

```
from urllib.parse import urlparse

result = urlparse(url)

print(result.scheme)
print(result.netloc)
```

Prefer URL parsing when you need actual URL components.

Don't write complicated regex for IP addresses when Python already provides a module.

```
import ipaddress

ip = "192.168.1.1"

try:
    address = ipaddress.ip_address(ip)
except ValueError:
    print("Invalid IP")
else:
    print("Valid IP:", address)
```

Also supports IPv6.

Don't use regex alone to determine whether a date actually exists.

Example:

```
from datetime import datetime

date_text = "2026-10-04"

try:
    date = datetime.strptime(
        date_text,
        "%Y-%m-%d"
    )
except ValueError:
    print("Invalid date")
else:
    print("Valid date:", date)
```

A regex can verify shape:

```
\d{4}-\d{2}-\d{2}
```

But it cannot by itself determine whether:

```
2026-99-99
```

is a real date.

A maintainable non-regex approach:

```
def validate_password(password):

    errors = []

    if len(password) < 8:
        errors.append(
            "At least 8 characters required"
        )

    if not any(
        c.isupper()
        for c in password
    ):
        errors.append(
            "Must contain uppercase letter"
        )

    if not any(
        c.islower()
        for c in password
    ):
        errors.append(
            "Must contain lowercase letter"
        )

    if not any(
        c.isdigit()
        for c in password
    ):
        errors.append(
            "Must contain a number"
        )

    if not any(
        not c.isalnum()
        for c in password
    ):
        errors.append(
            "Must contain special character"
        )

    return errors
```

Usage:

```
errors = validate_password("hello")

if errors:
    for error in errors:
        print(error)
else:
    print("Valid password")
```

```
import re


def validate_username(username):

    username = username.strip()

    if not username:
        return "Username is required"

    if not 3 <= len(username) <= 20:
        return "Username must be 3-20 characters"

    if not re.fullmatch(
        r"[A-Za-z0-9_]+",
        username
    ):
        return "Only letters, numbers and underscore allowed"

    if not username[0].isalpha():
        return "Username must start with a letter"

    return None
```

Usage:

```
error = validate_username("123john")

if error:
    print(error)
else:
    print("Valid username")
```

```
import re


def validate_registration(
    username,
    email,
    password,
    age
):

    errors = {}

    # -------------------------
    # USERNAME
    # -------------------------

    username = username.strip()

    if not username:
        errors["username"] = (
            "Username is required"
        )

    elif not 3 <= len(username) <= 20:
        errors["username"] = (
            "Username must be 3-20 characters"
        )

    elif not re.fullmatch(
        r"[A-Za-z0-9_]+",
        username
    ):
        errors["username"] = (
            "Invalid username characters"
        )


    # -------------------------
    # EMAIL
    # -------------------------

    email = email.strip()

    email_pattern = (
        r"[A-Za-z0-9._%+-]+"
        r"@"
        r"[A-Za-z0-9.-]+"
        r"\."
        r"[A-Za-z]{2,}"
    )

    if not re.fullmatch(
        email_pattern,
        email
    ):
        errors["email"] = (
            "Invalid email"
        )


    # -------------------------
    # PASSWORD
    # -------------------------

    if len(password) < 8:

        errors["password"] = (
            "Password must contain at least 8 characters"
        )

    elif not any(
        c.isupper()
        for c in password
    ):

        errors["password"] = (
            "Password needs uppercase"
        )

    elif not any(
        c.islower()
        for c in password
    ):

        errors["password"] = (
            "Password needs lowercase"
        )

    elif not any(
        c.isdigit()
        for c in password
    ):

        errors["password"] = (
            "Password needs a number"
        )


    # -------------------------
    # AGE
    # -------------------------

    try:

        age = int(age)

        if age < 18:
            errors["age"] = (
                "Must be 18 or older"
            )

    except ValueError:

        errors["age"] = (
            "Age must be a number"
        )


    return errors
```

Usage:

```
errors = validate_registration(
    "nzegge123",
    "nzegge@example.com",
    "Hello123!",
    "25"
)

if errors:
    print(errors)
else:
    print("Registration valid")
```

A good validation system usually follows this order:

```
1. Check type
2. Check required/empty
3. Normalize input
4. Check length
5. Check allowed characters
6. Check format
7. Check business rules
8. Convert data
9. Return errors or valid data
```

Example:

```
username = input("Username: ")


# 1. TYPE

if not isinstance(username, str):
    print("Must be text")


# 2. EMPTY

elif not username.strip():
    print("Username required")


else:

    # 3. NORMALIZE

    username = username.strip()


    # 4. LENGTH

    if not 3 <= len(username) <= 20:
        print("Invalid length")


    # 5. CHARACTERS

    elif not all(
        c.isalnum() or c == "_"
        for c in username
    ):
        print("Invalid characters")


    # 6. BUSINESS RULE

    elif not username[0].isalpha():
        print("Must start with letter")


    else:
        print("Valid username")
```

Do not use regex for everything.

Use normal Python when it is simpler.

---

## TYPE

Use:

```
isinstance(value, str)
```

---

## EMPTY

Use:

```
if not value:
```

---

## LENGTH

Use:

```
len(value)
```

---

## CONTAINS

Use:

```
"@" in value
```

---

## DOES NOT CONTAIN

Use:

```
" " not in value
```

---

## PREFIX

Use:

```
value.startswith("abc")
```

---

## SUFFIX

Use:

```
value.endswith(".jpg")
```

---

## DIGITS

Use:

```
value.isdigit()
```

---

## LETTERS

Use:

```
value.isalpha()
```

---

## LETTERS + NUMBERS

Use:

```
value.isalnum()
```

---

## AT LEAST ONE CONDITION

Use:

```
any(...)
```

---

## EVERY CONDITION

Use:

```
all(...)
```

---

## INTEGER

Use:

```
try:
    int(value)
except ValueError:
    ...
```

---

## FLOAT

Use:

```
try:
    float(value)
except ValueError:
    ...
```

---

## COMPLEX TEXT PATTERN

Use:

```
re.fullmatch()
re.search()
re.findall()
```

Don't use regex when Python already has a specialized tool.

---

## IP ADDRESS

```
import ipaddress
```

---

## DATE/TIME

```
from datetime import datetime
```

---

## URL

```
from urllib.parse import urlparse
```

---

## FILES/PATHS

```
from pathlib import Path
```

Example:

```
path = Path("photo.jpg")

if path.suffix.lower() in (
    ".jpg",
    ".png",
    ".webp"
):
    print("Image")
```

You can combine everything.

Example:

```
import re


def validate_username(username):

    # Normalize
    username = username.strip()

    # Required
    if not username:
        return False, "Username required"

    # Length
    if not 3 <= len(username) <= 20:
        return False, "Invalid length"

    # Pattern
    if not re.fullmatch(
        r"[A-Za-z0-9_]+",
        username
    ):
        return False, "Invalid characters"

    # First character
    if not username[0].isalpha():
        return False, "Must start with letter"

    # Reserved names
    blocked = {
        "admin",
        "root",
        "system"
    }

    if username.casefold() in blocked:
        return False, "Reserved username"

    return True, "Valid username"
```

Usage:

```
valid, message = validate_username(
    "nzegge123"
)

print(valid)
print(message)
```

CHARACTERS

```
.       almost any character

\d      digit
\D      non-digit

\w      word character
\W      non-word character

\s      whitespace
\S      non-whitespace
```

CHARACTER SETS

```
[abc]       a, b, or c

[^abc]      anything except a, b, c

[a-z]       lowercase

[A-Z]       uppercase

[0-9]       digits

[A-Za-z]    letters

[A-Za-z0-9] letters + numbers
```

QUANTIFIERS

```
*           zero or more

+           one or more

?           zero or one

{3}         exactly 3

{3,}        3 or more

{3,5}       3 to 5
```

ANCHORS

```
^           beginning

$           end

\b          word boundary

\B          not a word boundary
```

GROUPS

```
(...)       capturing group

(?:...)     non-capturing group

(?P<name>)  named group
```

ALTERNATION

```
|           OR
```

LOOKAROUND

```
(?=...)     positive lookahead

(?!...)     negative lookahead

(?<=...)    positive lookbehind

(?<!...)    negative lookbehind
```

LAZY

```
*?          lazy zero or more

+?          lazy one or more

??          lazy zero or one

{n,m}?      lazy range
```

```
re.search()
    Find pattern anywhere.


re.match()
    Match from beginning.


re.fullmatch()
    Entire string must match.


re.findall()
    Return all matches.


re.finditer()
    Return match objects.


re.split()
    Split using regex.


re.sub()
    Replace matches.


re.subn()
    Replace + count replacements.


re.compile()
    Compile reusable regex.


re.escape()
    Escape literal user input.
```

```
value.strip()
    Remove surrounding whitespace.


value.lower()
    Lowercase.


value.upper()
    Uppercase.


value.casefold()
    Strong case-insensitive normalization.


value.startswith("x")
    Starts with.


value.endswith("x")
    Ends with.


value.isdigit()
    Digits.


value.isdecimal()
    Decimal characters.


value.isnumeric()
    Numeric characters.


value.isalpha()
    Letters.


value.isalnum()
    Letters + numbers.


value.isspace()
    Whitespace.


value.islower()
    Lowercase.


value.isupper()
    Uppercase.


value.istitle()
    Title case.


value.isidentifier()
    Valid Python identifier.
```

```
isinstance()
    Check type.


len()
    Check length.


in
    Membership.


not in
    Non-membership.


any()
    At least one.


all()
    Every item.


callable()
    Can be called?


bool()
    Truthiness.


enumerate()
    Value + index.


zip()
    Combine sequences.


filter()
    Filter values.


try/except
    Handle conversion/operation failures.


assert
    Check programming assumptions.
```

Contains digit:

```
any(c.isdigit() for c in value)
```

Contains uppercase:

```
any(c.isupper() for c in value)
```

Contains lowercase:

```
any(c.islower() for c in value)
```

Contains whitespace:

```
any(c.isspace() for c in value)
```

Contains special character:

```
any(not c.isalnum() for c in value)
```

Contains one of specific characters:

```
any(c in "@#$%" for c in value)
```

Contains one of several words:

```
any(
    word in text
    for word in ["python", "django", "react"]
)
```

All characters are digits:

```
all(c.isdigit() for c in value)
```

All characters are alphanumeric:

```
all(c.isalnum() for c in value)
```

No whitespace:

```
all(not c.isspace() for c in value)
```

All characters are allowed:

```
allowed = "abc123_"

all(
    c.lower() in allowed
    for c in value
)
```

All numbers are positive:

```
all(
    number > 0
    for number in numbers
)
```

RULE 1:

Use the simplest tool that solves the problem.

Do not use regex just because you know regex.

RULE 2:

Use:

```
in
not in
startswith()
endswith()
isdigit()
isalpha()
isalnum()
len()
```

for simple validation.

RULE 3:

Use:

```
any()
```

when you need:

```
"Does AT LEAST ONE satisfy this?"
```

RULE 4:

Use:

```
all()
```

when you need:

```
"Do ALL satisfy this?"
```

RULE 5:

Use regex for complex TEXT PATTERNS.

RULE 6:

Use `re.fullmatch()` when the entire input must follow a regex pattern.

RULE 7:

Use specialized modules for specialized data.

Examples:

```
datetime
ipaddress
urllib.parse
pathlib
```

RULE 8:

Use `try/except` when conversion itself is the validation.

Example:

```
int()
float()
datetime.strptime()
```

RULE 9:

Normalize input before validating when appropriate.

Example:

```
username = username.strip()

email = email.strip().casefold()
```

RULE 10:

Separate validation rules into functions when your project becomes larger.

```
                VALIDATION
                    |
    +---------------+---------------+
    |               |               |
    v               v               v
  TYPE            VALUE           FORMAT
    |               |               |
```

isinstance()          len()          regex
in
not in
startswith()
endswith()
isdigit()
isalpha()
isalnum()
|
+-----+-----+
|           |
v           v
any()       all()
|           |
+-----+-----+
|
v
BUSINESS RULES
|
v
try / except
|
v
VALID DATA

If you want to become very strong at Python validation, master these first:

```
1. isinstance()

2. len()

3. in

4. not in

5. startswith()

6. endswith()

7. any()

8. all()

9. try/except

10. re.fullmatch()
```

Then learn:

```
re.search()
re.findall()
re.sub()
groups
lookaheads
lookbehinds
named groups
regex flags
compiled patterns
```

Here is a good example of combining multiple techniques:

```
import re


def validate_user(
    username,
    email,
    password,
    age
):

    errors = {}


    # -------------------------
    # USERNAME
    # -------------------------

    username = username.strip()

    if not username:
        errors["username"] = "Required"

    elif not 3 <= len(username) <= 20:
        errors["username"] = (
            "Must contain 3-20 characters"
        )

    elif not re.fullmatch(
        r"[A-Za-z0-9_]+",
        username
    ):
        errors["username"] = (
            "Only letters, numbers and underscore"
        )

    elif not username[0].isalpha():
        errors["username"] = (
            "Must start with a letter"
        )


    # -------------------------
    # EMAIL
    # -------------------------

    email = email.strip().casefold()

    email_pattern = (
        r"[A-Za-z0-9._%+-]+"
        r"@"
        r"[A-Za-z0-9.-]+"
        r"\."
        r"[A-Za-z]{2,}"
    )

    if not re.fullmatch(
        email_pattern,
        email
    ):
        errors["email"] = "Invalid email"


    # -------------------------
    # PASSWORD
    # -------------------------

    if len(password) < 8:
        errors["password"] = (
            "Minimum 8 characters"
        )

    elif not any(
        c.isupper()
        for c in password
    ):
        errors["password"] = (
            "Must contain uppercase"
        )

    elif not any(
        c.islower()
        for c in password
    ):
        errors["password"] = (
            "Must contain lowercase"
        )

    elif not any(
        c.isdigit()
        for c in password
    ):
        errors["password"] = (
            "Must contain a number"
        )

    elif not any(
        not c.isalnum()
        for c in password
    ):
        errors["password"] = (
            "Must contain special character"
        )


    # -------------------------
    # AGE
    # -------------------------

    try:

        age = int(age)

        if not 18 <= age <= 120:
            errors["age"] = (
                "Age must be between 18 and 120"
            )

    except ValueError:

        errors["age"] = (
            "Age must be a valid number"
        )


    return errors


# -------------------------
# TEST
# -------------------------

errors = validate_user(
    username="nzegge123",
    email="NZEgge@example.com",
    password="Hello123!",
    age="25"
)


if errors:

    print("Validation failed:")

    for field, error in errors.items():
        print(f"{field}: {error}")

else:

    print("Everything is valid!")
```
