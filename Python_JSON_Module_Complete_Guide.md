# Python `json` Module — Practical Reference Guide

## 1. What is JSON?

JSON means **JavaScript Object Notation**. It is a text format used to store and exchange structured data between programs. Python includes the `json` module in its standard library, so no installation is required.

```python
import json
```

Python example:

```python
user = {
    "username": "nzegge",
    "active": True,
    "age": 25,
    "skills": ["Python", "Networking"],
    "address": None,
}
```

Equivalent JSON:

```json
{
    "username": "nzegge",
    "active": true,
    "age": 25,
    "skills": ["Python", "Networking"],
    "address": null
}
```

JSON uses lowercase `true`, `false`, and `null`; Python uses `True`, `False`, and `None`.

## 2. Python types and JSON equivalents

| Python | JSON | Parsed Python result |
|---|---|---|
| `dict` | object `{}` | `dict` |
| `list` | array `[]` | `list` |
| `str` | string | `str` |
| `int` | number | `int` |
| `float` | number | `float` |
| `True` | `true` | `True` |
| `False` | `false` | `False` |
| `None` | `null` | `None` |

JSON does not directly support `tuple`, `set`, `bytes`, `complex`, `datetime`, `Decimal`, `Path`, or arbitrary custom objects. Decide how to represent these before encoding.

A tuple becomes a JSON array and comes back as a list:

```python
import json

value = (10, 20, 30)
encoded = json.dumps(value)
decoded = json.loads(encoded)

print(encoded)          # [10, 20, 30]
print(type(decoded))    # <class 'list'>
```

A set is not supported automatically:

```python
# json.dumps({1, 2, 3})  # TypeError
print(json.dumps(sorted({1, 2, 3})))
```

## 3. The four essential functions

| Function | Input | Output | Purpose |
|---|---|---|---|
| `json.dumps()` | Python object | JSON string (`str`) | Serialize to text |
| `json.loads()` | JSON text | Python object | Deserialize text |
| `json.dump()` | Python object + writable file | Writes JSON | Serialize to file |
| `json.load()` | Readable file | Python object | Read and deserialize file |

Memory aid:

```text
dumps = Python object -> JSON string
loads = JSON string   -> Python object

dump  = Python object -> JSON file
load  = JSON file     -> Python object
```

The `s` in `dumps` and `loads` reminds you that these functions work with strings.

## 4. `json.dumps()` — Python object to JSON string

```python
import json

user = {"username": "nzegge", "age": 25, "active": True}

json_text = json.dumps(user)

print(json_text)
print(type(json_text))  # <class 'str'>
```

Possible output:

```text
{"username": "nzegge", "age": 25, "active": true}
<class 'str'>
```

The result is a string, not a dictionary.

### Syntax

```python
json.dumps(
    obj,
    *,
    skipkeys=False,
    ensure_ascii=True,
    check_circular=True,
    allow_nan=True,
    cls=None,
    indent=None,
    separators=None,
    default=None,
    sort_keys=False,
    **kw
)
```

The `*` means the parameters after it are keyword-only.

### `indent`

Controls human-readable indentation.

```python
print(json.dumps(user, indent=4))
```

- `indent=None`: default compact-ish formatting.
- `indent=2`: indent nested levels by two spaces.
- `indent=4`: indent nested levels by four spaces.
- `indent="\\t"`: use a tab for indentation.

Use indentation for configuration files and debugging. Compact output is often better for network payloads.

### `sort_keys`

Sorts dictionary/object keys, not list elements.

```python
data = {"zebra": 1, "apple": 2, "mango": 3}
print(json.dumps(data, sort_keys=True))
# {"apple": 2, "mango": 3, "zebra": 1}
```

Useful for predictable output, tests, and readable diffs.

### `ensure_ascii`

Controls how non-ASCII characters are represented.

```python
person = {"name": "André", "city": "Buea"}

print(json.dumps(person, ensure_ascii=True))
print(json.dumps(person, ensure_ascii=False))
```

With `ensure_ascii=True`, characters may be escaped:

```json
{"name": "Andr\u00e9", "city": "Buea"}
```

With `ensure_ascii=False`, they remain readable:

```json
{"name": "André", "city": "Buea"}
```

Both are valid JSON. For UTF-8 files and APIs, `ensure_ascii=False` is often convenient.

### `separators`

Controls separators between items and between keys and values.

```python
data = {"name": "Nzegge", "age": 25}

print(json.dumps(data, separators=(", ", ": ")))
print(json.dumps(data, separators=(",", ":")))
```

Output:

```text
{"name": "Nzegge", "age": 25}
{"name":"Nzegge","age":25}
```

Compact separators are `(",", ":")`. The first value separates items; the second separates a key from its value.

### `skipkeys`

By default, JSON object keys must be strings, numbers, booleans, or `None` (non-string supported keys are converted to strings). Other key types raise `TypeError`.

```python
data = {"username": "nzegge", (1, 2): "coordinate"}

# json.dumps(data)  # TypeError: tuple is not a valid JSON key
print(json.dumps(data, skipkeys=True))
```

Use `skipkeys=True` cautiously: unsupported keys are silently discarded. Usually it is better to convert the data deliberately.

### `allow_nan`

Python's encoder permits `NaN`, `Infinity`, and `-Infinity` by default, although these are not valid standard JSON numbers.

```python
data = {"measurement": float("nan")}
print(json.dumps(data))  # {"measurement": NaN}
```

For strict JSON:

```python
json.dumps(data, allow_nan=False)  # raises ValueError
```

Consider `allow_nan=False` for interoperable APIs and files.

### `check_circular`

The default is `True`. It checks containers for circular references.

```python
data = []
data.append(data)  # The list contains itself.

# json.dumps(data)  # ValueError: circular reference detected
```

Do not disable this unless you have a specific reason and know the structure cannot contain cycles.

### `default`

`default` is a function called when the encoder encounters an unsupported object.

```python
import json
from pathlib import Path

data = {"project": "network-lab", "path": Path("logs/app.log")}

def convert_object(value):
    if isinstance(value, Path):
        return str(value)

    raise TypeError(
        f"Object of type {type(value).__name__} is not JSON serializable"
    )

json_text = json.dumps(data, default=convert_object, indent=4)
print(json_text)
```

The function must return a JSON-serializable value: string, number, list, dictionary, boolean, or `None`. Do not return the same unsupported object.

## 5. `json.loads()` — JSON text to Python object

```python
import json

json_text = '''
{
    "username": "nzegge",
    "age": 25,
    "active": true,
    "skills": ["Python", "SQL"],
    "address": null
}
'''

user = json.loads(json_text)

print(user)
print(type(user))          # <class 'dict'>
print(user["username"])    # nzegge
print(user["active"])      # True
```

It can also parse a JSON string, number, boolean, null, array, or object:

```python
json.loads('{"name": "Nzegge"}')  # dict
json.loads('[10, 20, 30]')         # list
json.loads('"hello"')              # str
json.loads('25')                   # int
json.loads('3.14')                 # float
json.loads('true')                 # True
json.loads('false')                # False
json.loads('null')                 # None
```

### Valid JSON syntax

JSON requires double quotes around strings and object keys. These are invalid:

```text
{'name': 'Nzegge'}       single quotes
{"active": True}         Python True, not JSON true
{"address": None}        Python None, not JSON null
{"age": 25,}             trailing comma
{ name: "Nzegge" }       unquoted key
```

Correct:

```python
json.loads('{"name": "Nzegge", "active": true, "address": null}')
```

### Handle invalid JSON

Malformed JSON raises `json.JSONDecodeError`, a subclass of `ValueError`.

```python
import json

json_text = '{"username": "nzegge", "age": }'

try:
    user = json.loads(json_text)
except json.JSONDecodeError as error:
    print("Invalid JSON:", error)
    print("Message:", error.msg)
    print("Line:", error.lineno)
    print("Column:", error.colno)
    print("Character position:", error.pos)
```

Useful attributes: `msg`, `doc`, `pos`, `lineno`, and `colno`.

## 6. `json.dump()` — Write Python data to a file

`dump()` writes JSON text to an open, writable file-like object.

```python
import json

settings = {
    "theme": "dark",
    "font_size": 16,
    "notifications": True,
}

with open("settings.json", "w", encoding="utf-8") as file:
    json.dump(settings, file, indent=4, ensure_ascii=False)
```

Use `encoding="utf-8"` for predictable text handling. `dump()` does not automatically add a newline:

```python
with open("settings.json", "w", encoding="utf-8") as file:
    json.dump(settings, file, indent=4)
    file.write("\\n")
```

## 7. `json.load()` — Read JSON from a file

```python
import json

with open("settings.json", "r", encoding="utf-8") as file:
    settings = json.load(file)

print(settings)
print(settings["theme"])
```

### Full write-and-read example

```python
import json

users = [
    {"id": 1, "username": "nzegge", "active": True},
    {"id": 2, "username": "alex", "active": False},
]

with open("users.json", "w", encoding="utf-8") as file:
    json.dump(users, file, indent=4)

with open("users.json", "r", encoding="utf-8") as file:
    loaded_users = json.load(file)

for user in loaded_users:
    print(user["username"], user["active"])
```

## 8. Custom objects

### Convert a `Path`

```python
import json
from pathlib import Path

data = {"log_file": Path("logs/app.log")}

def json_default(value):
    if isinstance(value, Path):
        return str(value)
    raise TypeError(f"Unsupported type: {type(value).__name__}")

print(json.dumps(data, default=json_default))
```

### Convert a `datetime`

ISO 8601 is a common representation:

```python
import json
from datetime import datetime, timezone

data = {"created_at": datetime.now(timezone.utc)}

def json_default(value):
    if isinstance(value, datetime):
        return value.isoformat()
    raise TypeError(f"Unsupported type: {type(value).__name__}")

encoded = json.dumps(data, default=json_default, indent=4)
print(encoded)
```

The date is stored as a JSON string. `json.loads()` will not automatically convert it back to `datetime`:

```python
from datetime import datetime

date_text = "2026-11-05T16:00:00+00:00"
created_at = datetime.fromisoformat(date_text)
```

### Convert a dataclass

```python
import json
from dataclasses import asdict, dataclass

@dataclass
class User:
    username: str
    active: bool

user = User("nzegge", True)
json_text = json.dumps(asdict(user), indent=4)
print(json_text)
```

`asdict()` converts the dataclass to ordinary dictionaries and lists.

## 9. Custom decoding with `object_hook`

`object_hook` is called for each decoded JSON object (dictionary). It receives a dictionary and returns the replacement value.

```python
import json
from dataclasses import dataclass

@dataclass
class User:
    username: str
    active: bool

def decode_user(data):
    if data.get("__type__") == "User":
        return User(data["username"], data["active"])
    return data

json_text = '''
{
    "__type__": "User",
    "username": "nzegge",
    "active": true
}
'''

user = json.loads(json_text, object_hook=decode_user)
print(user)
print(type(user))
```

Use an explicit marker so ordinary dictionaries are not accidentally converted. Never let untrusted JSON select arbitrary Python classes or functions to instantiate.

## 10. `object_pairs_hook` — Inspect object pairs and duplicate keys

JSON object names should be unique, but malformed or ambiguous documents may contain duplicates. A normal dictionary cannot preserve two values for one key.

`object_pairs_hook` receives an ordered list of key/value pairs. If both `object_pairs_hook` and `object_hook` are supplied, `object_pairs_hook` takes precedence.

```python
import json

json_text = '{"role": "user", "role": "admin"}'

pairs = json.loads(
    json_text,
    object_pairs_hook=lambda items: items
)

print(pairs)
# [('role', 'user'), ('role', 'admin')]
```

Reject duplicates:

```python
import json

def reject_duplicates(pairs):
    result = {}

    for key, value in pairs:
        if key in result:
            raise ValueError(f"Duplicate JSON key: {key}")
        result[key] = value

    return result

json_text = '{"username": "nzegge", "username": "admin"}'

try:
    data = json.loads(json_text, object_pairs_hook=reject_duplicates)
except ValueError as error:
    print(error)
```

## 11. Custom number parsing

### `parse_int`

Called for JSON integer tokens:

```python
import json

data = json.loads('{"id": 123}', parse_int=str)
print(data)               # {'id': '123'}
print(type(data["id"]))   # str
```

This is an illustration; normally keep integer values as integers.

### `parse_float`

Called for JSON floating-point tokens. It can be useful for decimal arithmetic:

```python
import json
from decimal import Decimal

data = json.loads('{"price": 19.99}', parse_float=Decimal)

print(data["price"])
print(type(data["price"]))  # <class 'decimal.Decimal'>
```

The standard encoder does not automatically serialize `Decimal`. Choose an explicit representation, often a string, when encoding.

### `parse_constant`

Called for the non-standard tokens `NaN`, `Infinity`, and `-Infinity` accepted by Python's decoder.

```python
import json

def reject_constant(value):
    raise ValueError(f"Invalid JSON numeric constant: {value}")

try:
    json.loads('{"value": NaN}', parse_constant=reject_constant)
except ValueError as error:
    print(error)
```

## 12. `JSONEncoder` — Custom encoding class

For reusable custom serialization, subclass `json.JSONEncoder`.

```python
import json
from pathlib import Path

class CustomEncoder(json.JSONEncoder):
    def default(self, value):
        if isinstance(value, Path):
            return str(value)
        return super().default(value)

data = {"config_path": Path("config/settings.json")}

json_text = json.dumps(data, cls=CustomEncoder, indent=4)
print(json_text)
```

`super().default(value)` preserves the standard behaviour for unsupported objects: it raises `TypeError`.

Useful `JSONEncoder` methods:

- `default(obj)`: override to convert unsupported objects.
- `encode(obj)`: encode an object into a JSON string.
- `iterencode(obj)`: yield encoded text chunks.

Example:

```python
encoder = json.JSONEncoder(indent=2)

for chunk in encoder.iterencode({"name": "Nzegge", "active": True}):
    print(repr(chunk))
```

## 13. `JSONDecoder` — Custom decoding class

`JSONDecoder` is the class used to decode JSON.

```python
import json

decoder = json.JSONDecoder(object_hook=lambda obj: obj)
data = decoder.decode('{"username": "nzegge"}')

print(data)
```

Useful methods:

- `decode(s)`: decode a complete JSON document.
- `raw_decode(s)`: decode one JSON document from the beginning of a string and return `(object, end_index)`.

Example:

```python
import json

decoder = json.JSONDecoder()
text = '{"id": 1}{"id": 2}'

first_object, end_position = decoder.raw_decode(text)

print(first_object)
print(end_position)
print(text[end_position:])
```

`raw_decode()` is a lower-level tool. For ordinary files and API responses, prefer `json.loads()`.

## 14. JSON and HTTP APIs

JSON is a common request and response format in REST APIs.

Example response:

```json
{
    "id": 1,
    "username": "nzegge",
    "is_active": true
}
```

With the third-party `requests` library:

```python
import requests

response = requests.get(
    "https://example.com/api/users/1",
    timeout=10,
)

response.raise_for_status()
data = response.json()
print(data)
```

`response.json()` belongs to `requests`, not the built-in `json` module. If you already have response text, you can use:

```python
import json

data = json.loads(response.text)
```

Create a JSON request:

```python
import requests

payload = {"username": "nzegge", "active": True}

response = requests.post(
    "https://example.com/api/users/",
    json=payload,
    timeout=10,
)

response.raise_for_status()
print(response.status_code)
```

The `json=` parameter is a `requests` convenience that serializes the Python data and sets the JSON content type.

## 15. JSON over TCP sockets

TCP is a byte stream; it does not preserve application message boundaries. One `recv()` call is not guaranteed to return one complete JSON message.

For a small local exercise, use newline-delimited JSON.

Sender:

```python
import json
import socket

message = {"type": "greeting", "username": "nzegge"}

json_text = json.dumps(message)
payload = (json_text + "\\n").encode("utf-8")

with socket.create_connection(("127.0.0.1", 5000), timeout=5) as client:
    client.sendall(payload)
```

Receiver:

```python
import json
import socket

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as server:
    server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server.bind(("127.0.0.1", 5000))
    server.listen(1)

    connection, address = server.accept()

    with connection:
        file = connection.makefile("r", encoding="utf-8")

        for line in file:
            message = json.loads(line)
            print("Received:", message)
```

This newline-delimited approach is often called **JSON Lines** or **NDJSON** when each line is a separate JSON value. A production protocol must also handle line-size limits, malformed messages, timeouts, and partial network delivery.

## 16. JSON Lines (NDJSON)

Ordinary JSON represents one complete JSON value. JSON Lines stores one JSON value per line.

Example `events.jsonl`:

```json
{"event": "login", "user": "nzegge"}
{"event": "logout", "user": "nzegge"}
{"event": "login", "user": "alex"}
```

Read line by line:

```python
import json

with open("events.jsonl", "r", encoding="utf-8") as file:
    for line_number, line in enumerate(file, start=1):
        if not line.strip():
            continue

        try:
            event = json.loads(line)
        except json.JSONDecodeError as error:
            print(f"Invalid JSON on line {line_number}: {error}")
            continue

        print(event)
```

Write JSON Lines:

```python
import json

events = [
    {"event": "login", "user": "nzegge"},
    {"event": "logout", "user": "nzegge"},
]

with open("events.jsonl", "w", encoding="utf-8") as file:
    for event in events:
        file.write(json.dumps(event) + "\\n")
```

Do not repeatedly call `json.dump()` on multiple objects in one ordinary JSON file without defining a framing format. The concatenated output may not be one valid JSON document.

## 17. Common errors

### `TypeError: Object of type X is not JSON serializable`

Cause: the object has no built-in JSON representation.

```python
from pathlib import Path
import json

# json.dumps({"path": Path("data.txt")})  # TypeError
print(json.dumps({"path": str(Path("data.txt"))}))
```

Fix: convert the value deliberately or use `default=` / a custom encoder.

### `json.JSONDecodeError`

Cause: malformed JSON.

```python
# Invalid:
# json.loads("{'name': 'Nzegge'}")

# Valid:
json.loads('{"name": "Nzegge"}')
```

### `ValueError` with `allow_nan=False`

Cause: the data contains `NaN` or infinity. Validate or replace non-finite numbers before encoding:

```python
import math

value = float("nan")

if not math.isfinite(value):
    value = None
```

### `FileNotFoundError`

Cause: the path does not exist. Check the current working directory and resolved path:

```python
from pathlib import Path

path = Path("settings.json")
print(path.resolve())
print(path.exists())
```

### `UnicodeDecodeError`

Cause: the file's bytes do not match the selected encoding. Identify the actual encoding; do not guess if the source is unknown. Use UTF-8 for JSON files you create.

### `Extra data`

Cause: `json.loads()` received more than one JSON document:

```python
# json.loads('{"a": 1}{"b": 2}')  # JSONDecodeError
```

Fix: use a defined format such as JSON Lines or a suitable streaming/framing parser.

## 18. Security and best practices

1. Treat JSON input as untrusted. Valid syntax does not mean the data is safe or meaningful.
2. Validate data after parsing: required keys, types, lengths, ranges, and allowed values.
3. Limit input size. Very large or deeply nested JSON can consume CPU and memory.
4. Never use `eval()` to parse JSON. Use `json.loads()`.
5. Remember JSON uses `true`, `false`, `null`, and double-quoted strings.
6. Use UTF-8 explicitly for files.
7. Use `indent=4` for readable configuration files.
8. Use compact separators when compact output is useful.
9. Consider `allow_nan=False` when strict standard JSON is required.
10. Handle `JSONDecodeError` when parsing external data.
11. Do not treat object key order as a security feature.
12. Do not store passwords or secrets in plain JSON configuration files.
13. Do not trust client-supplied fields such as `role`, `is_admin`, or `user_id` for authorization. Enforce permissions on the server.
14. Keep `object_hook` logic explicit and restricted.
15. Use a schema or validation layer when the data structure is important. Parsing checks syntax, not business rules.

Parsing is not validation. This is valid JSON:

```json
{
    "username": 123,
    "age": "not-a-number"
}
```

But an application may require `username` to be a string and `age` to be an integer.

```python
import json

data = json.loads('{"username": 123, "age": "not-a-number"}')

if not isinstance(data, dict):
    raise ValueError("Expected a JSON object")

if not isinstance(data.get("username"), str):
    raise ValueError("username must be a string")

if not isinstance(data.get("age"), int):
    raise ValueError("age must be an integer")
```

## 19. Quick reference

### Main functions

| Name | Purpose |
|---|---|
| `json.dump(obj, fp, ...)` | Write Python data as JSON to a file-like object |
| `json.dumps(obj, ...)` | Return JSON text from Python data |
| `json.load(fp, ...)` | Read JSON from a file-like object |
| `json.loads(s, ...)` | Parse JSON text |
| `json.detect_encoding(b)` | Determine encoding of JSON byte input |

### Main classes

| Name | Purpose |
|---|---|
| `json.JSONEncoder` | Encodes Python objects as JSON |
| `json.JSONDecoder` | Decodes JSON into Python objects |

### Encoder options

| Option | Meaning |
|---|---|
| `skipkeys` | Skip unsupported dictionary key types instead of raising `TypeError` |
| `ensure_ascii` | Escape non-ASCII characters when true |
| `check_circular` | Check for circular container references |
| `allow_nan` | Allow non-standard NaN and infinity tokens |
| `cls` | Use a custom `JSONEncoder` subclass |
| `indent` | Add readable indentation |
| `separators` | Customize separators |
| `default` | Convert unsupported Python objects |
| `sort_keys` | Sort object keys |

### Decoder options

| Option | Meaning |
|---|---|
| `cls` | Use a custom `JSONDecoder` subclass |
| `object_hook` | Transform each decoded JSON object |
| `object_pairs_hook` | Transform ordered object key/value pairs |
| `parse_float` | Customize parsing of JSON floating-point numbers |
| `parse_int` | Customize parsing of JSON integers |
| `parse_constant` | Handle `NaN`, `Infinity`, and `-Infinity` tokens |

### Important exceptions

| Exception | Typical cause |
|---|---|
| `json.JSONDecodeError` | Invalid JSON text |
| `TypeError` | Unsupported Python object or invalid argument type |
| `ValueError` | Invalid value, including non-finite numbers with `allow_nan=False` |
| `OSError` | File or operating-system error |

## 20. Mini-project: JSON settings manager

This example loads a settings file, uses defaults if it does not exist, validates basic values, and saves settings.

```python
import json
from pathlib import Path

SETTINGS_FILE = Path("settings.json")

DEFAULT_SETTINGS = {
    "username": "guest",
    "theme": "dark",
    "notifications": True,
}

def load_settings() -> dict:
    # Load settings, or return a copy of the defaults.
    if not SETTINGS_FILE.exists():
        return DEFAULT_SETTINGS.copy()

    try:
        with SETTINGS_FILE.open("r", encoding="utf-8") as file:
            settings = json.load(file)
    except json.JSONDecodeError as error:
        raise ValueError(
            f"Settings file contains invalid JSON: {error}"
        ) from error

    if not isinstance(settings, dict):
        raise ValueError("The settings file must contain a JSON object.")

    if not isinstance(settings.get("username"), str):
        raise ValueError("username must be a string")

    if settings.get("theme") not in {"dark", "light"}:
        raise ValueError("theme must be 'dark' or 'light'")

    if not isinstance(settings.get("notifications"), bool):
        raise ValueError("notifications must be a boolean")

    return settings

def save_settings(settings: dict) -> None:
    # Save settings as readable UTF-8 JSON.
    temporary_file = SETTINGS_FILE.with_suffix(".tmp")

    with temporary_file.open("w", encoding="utf-8") as file:
        json.dump(
            settings,
            file,
            indent=4,
            ensure_ascii=False,
            allow_nan=False,
        )
        file.write("\\n")

    # Replace the old file after the temporary file has been written.
    temporary_file.replace(SETTINGS_FILE)

settings = load_settings()
print("Current settings:", settings)

settings["username"] = "nzegge"
settings["theme"] = "light"

save_settings(settings)
print("Settings saved.")
```

The temporary-file approach helps avoid leaving a partially written settings file if writing fails before replacement. Stronger crash durability may require filesystem syncing.

## 21. Practice exercises

Try these without looking at the examples first:

1. Create a dictionary containing your username, age, country, and programming languages.
2. Convert it to JSON text using `json.dumps()`.
3. Print it with `indent=4`.
4. Convert it back using `json.loads()`.
5. Save it to `profile.json` using `json.dump()`.
6. Read `profile.json` using `json.load()`.
7. Add a nested `network` dictionary with an IP address and port.
8. Sort JSON object keys using `sort_keys=True`.
9. Parse invalid JSON and handle `JSONDecodeError`.
10. Serialize a `Path` object using `default=`.
11. Parse a decimal value with `parse_float=Decimal`.
12. Write three JSON objects to a `.jsonl` file and read them line by line.
13. Write a validator that rejects a profile if `username` is not a string or `age` is not an integer.

## 22. Official documentation

- Python `json` module: https://docs.python.org/3/library/json.html
- JSON data interchange standard (RFC 8259): https://www.rfc-editor.org/rfc/rfc8259

## Final mental model

```text
Python object
     |
     | json.dumps()
     v
JSON string
     |
     | json.loads()
     v
Python object
```

For files:

```text
Python object
     |
     | json.dump()
     v
JSON file
     |
     | json.load()
     v
Python object
```

Master these four functions first. Then practise `default`, `object_hook`, `JSONEncoder`, and `JSONDecoder`.
