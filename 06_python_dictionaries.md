# Python Dictionaries (`dict`) — Keys, Values, and Practical Patterns

## 1. What is a dictionary?
A dictionary maps unique, hashable keys to values. It is mutable and preserves insertion order in modern Python.

```python
student = {
    "name": "Charles",
    "age": 21,
    "active": True,
}
```

## 2. Read values safely

```python
print(student["name"])           # raises KeyError if missing
print(student.get("email"))      # None if missing
print(student.get("email", ""))  # supplied default
```

Use `[]` when a missing key should be an error. Use `get()` when a missing key is an expected possibility.

## 3. Add, update, and remove

```python
student["email"] = "charles@example.com"
student["age"] = 22
student.update({"active": False, "city": "Buea"})

email = student.pop("email", None)
last_item = student.popitem()
student.setdefault("country", "Cameroon")
```

- `update()` merges supplied key/value pairs.
- `pop(key, default)` removes and returns a key's value.
- `popitem()` removes and returns the most recently inserted pair.
- `setdefault()` returns the current value or inserts a default if absent.
- `del student[key]` removes a key but raises `KeyError` if absent.
- `clear()` removes all entries.

## 4. Iterate through dictionaries

```python
for key in student:
    print(key)

for value in student.values():
    print(value)

for key, value in student.items():
    print(key, value)
```

Use `.items()` when you need both keys and values.

## 5. Keys must be hashable

Strings, integers, and tuples of hashable values can be keys. Lists and dictionaries cannot be keys because they are mutable and unhashable.

```python
lookup = {
    "user": 1,
    42: "answer",
    ("x", "y"): "coordinate",
}
```

## 6. Dictionary comprehensions

```python
squares = {number: number ** 2 for number in range(1, 6)}
print(squares)
```

Filter while creating:
```python
even_squares = {
    n: n * n
    for n in range(10)
    if n % 2 == 0
}
```

## 7. Merge dictionaries

```python
defaults = {"timeout": 10, "retries": 3}
custom = {"timeout": 20}

settings = defaults | custom  # Python 3.9+
print(settings)  # custom value overrides default

defaults.update(custom)       # mutates defaults
```

`|` creates a new dictionary; `update()` modifies the existing dictionary.

## 8. Nested dictionaries

```python
users = {
    1: {"name": "Charles", "active": True},
    2: {"name": "Amina", "active": False},
}

print(users[1]["name"])
```

For deeply nested or externally supplied data, validate keys and types rather than assuming every level exists.

## 9. Count values with `Counter`

```python
from collections import Counter

counts = Counter(["python", "sql", "python", "linux"])
print(counts["python"])  # 2
```

`Counter` is a dictionary-like tool for counting hashable items.

## 10. Common mistakes and best practices

- Accessing a possibly missing key with `[]` without handling `KeyError`.
- Assuming dictionaries can use lists as keys.
- Modifying a dictionary while iterating over it; iterate over a copy or collect changes first.
- Using `setdefault()` when a simple assignment or `get()` is clearer.
- Sharing a mutable nested dictionary unintentionally; shallow copies do not recursively copy nested objects.
- Storing passwords or sensitive tokens in plain-text dictionaries longer than necessary.

## 11. Quick reference

| Task | Pattern |
|---|---|
| Read required key | `d[key]` |
| Read optional key | `d.get(key, default)` |
| Add/update | `d[key] = value` |
| Update many | `d.update(other)` |
| Remove and return | `d.pop(key, default)` |
| Iterate pairs | `d.items()` |
| Iterate keys/values | `d.keys()`, `d.values()` |
| Create from iterable | `{k: v for ...}` |
| Merge (Python 3.9+) | `d1 \| d2` |

## 12. Practice
1. Build a dictionary for a student.
2. Count repeated words with `Counter`.
3. Make a dictionary comprehension of numbers and squares.
4. Merge default settings with user settings.
5. Safely read a possibly missing key.

**Key idea:** dictionaries provide fast key-based lookup and are ideal for structured mappings such as settings, records, and JSON-like data.
