# Python Strings (`str`) — A Practical Guide

## 1. What is a string?
A string (`str`) is an immutable sequence of Unicode characters.

```python
name = "Charles"
message = 'Hello'
multiline = """Line one
Line two"""
```

Strings are immutable: operations create new strings rather than changing the original string in place.

## 2. Indexing and slicing

```python
text = "Python"
print(text[0])     # P
print(text[-1])    # n
print(text[0:3])   # Pyt
print(text[::2])   # Pto
print(text[::-1])  # nohtyP
```

Slices use `text[start:stop:step]`; the `stop` index is excluded. Invalid single indexes raise `IndexError`, while out-of-range slice bounds are usually allowed.

## 3. Common string methods

```python
text = "  Hello, Python!  "

print(text.strip())
print(text.lower())
print(text.upper())
print(text.title())
print(text.replace("Python", "World"))
print("a,b,c".split(","))
print("-".join(["a", "b", "c"]))
```

Useful methods:
- Case: `lower()`, `upper()`, `casefold()`, `capitalize()`, `title()`, `swapcase()`.
- Trim: `strip()`, `lstrip()`, `rstrip()`.
- Search: `find()`, `rfind()`, `index()`, `count()`, `startswith()`, `endswith()`.
- Replace/split: `replace()`, `split()`, `rsplit()`, `splitlines()`, `partition()`, `rpartition()`.
- Tests: `isalpha()`, `isdigit()`, `isdecimal()`, `isnumeric()`, `isalnum()`, `isspace()`, `islower()`, `isupper()`.
- Alignment: `center()`, `ljust()`, `rjust()`, `zfill()`.
- Formatting: `format()`, `format_map()`.

`find()` returns `-1` when absent; `index()` raises `ValueError`. These methods are case-sensitive unless you normalize the text.

## 4. f-strings and formatting

```python
name = "Charles"
score = 92.456
print(f"{name} scored {score:.2f}")
print("Hello, {}".format(name))
```

F-strings are generally the clearest choice for inserting values into text. Formatting specifiers can control decimal places, alignment, width, and number representation.

## 5. String membership and iteration

```python
text = "cybersecurity"

print("security" in text)  # True

for character in text:
    print(character)
```

Use `in` for substring checks. Remember that strings are case-sensitive by default.

## 6. String and bytes are different

```python
text = "café"
data = text.encode("utf-8")  # str -> bytes
restored = data.decode("utf-8")  # bytes -> str

print(data)
print(restored)
```

Encoding converts text to bytes using a chosen encoding. Decoding converts bytes to text. Arbitrary binary data may not decode as UTF-8.

Hex representation:
```python
hex_text = data.hex()
same_bytes = bytes.fromhex(hex_text)
```
Hex is a representation, not encryption.

## 7. Unicode and case-insensitive comparison

```python
a = "Straße"
print(a.casefold())
```

`casefold()` is intended for caseless matching and can be more thorough than `lower()` for some Unicode text. Unicode normalization may also matter when comparing visually similar text represented with different code-point sequences.

## 8. Safe handling of untrusted text

- Use parameterized SQL queries instead of inserting strings into SQL with f-strings.
- Escape output appropriately for the destination context (HTML, shell, SQL, etc.).
- Do not assume a string is safe just because it has been stripped or checked with a regex.
- Use `pathlib` or safe process APIs rather than building shell commands from untrusted strings.

## 9. Common mistakes

- Trying to change a string in place.
- Confusing `find()` and `index()`.
- Forgetting that the slice stop is excluded.
- Treating bytes as strings.
- Using `isnumeric()` when only ASCII digits are allowed.
- Building SQL statements with string concatenation or f-strings.

## 10. Practice
1. Extract the first and last character of a string.
2. Count a substring using `count()`.
3. Normalize whitespace using `split()` and `" ".join(...)`.
4. Encode a Unicode string to UTF-8 and decode it.
5. Explain why f-strings should not be used to insert untrusted values into SQL.

**Key idea:** strings are immutable Unicode text; choose methods according to whether you need search, transform, format, or encode.
