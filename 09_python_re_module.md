# Python `re` Module — Regular Expressions

## 1. What is `re`?
Python's built-in `re` module searches, validates, splits, and replaces text using patterns called regular expressions (regex).

```python
import re

text = "Contact me at student@example.com"
match = re.search(r"[\w.-]+@[\w.-]+\.\w+", text)
if match:
    print(match.group())
```

The pattern uses `r"..."` (a raw string) so backslashes are passed to the regex engine without Python interpreting them first.

## 2. Core functions

- `re.search(pattern, string)`: finds the first match anywhere.
- `re.match(pattern, string)`: checks only at the start.
- `re.fullmatch(pattern, string)`: requires the entire string to match.
- `re.findall(pattern, string)`: returns all non-overlapping matches.
- `re.finditer(pattern, string)`: returns match objects for each match.
- `re.sub(pattern, replacement, string)`: replaces matches.
- `re.split(pattern, string)`: splits on matches.
- `re.compile(pattern, flags=0)`: compiles a reusable pattern.
- `re.escape(string)`: escapes text so it can be treated literally in a regex.

## 3. Pattern building blocks

| Pattern | Meaning |
|---|---|
| `.` | Any character except newline by default |
| `^` | Start of string/line depending on flags |
| `$` | End of string/line depending on flags |
| `*` | Zero or more repetitions |
| `+` | One or more repetitions |
| `?` | Zero or one repetition |
| `{3}` | Exactly 3 repetitions |
| `{2,5}` | Between 2 and 5 repetitions |
| `[abc]` | One of a, b, c |
| `[A-Z]` | Uppercase ASCII letter range |
| `[^0-9]` | A character not in 0–9 |
| `\d` | Digit (Unicode-aware by default) |
| `\w` | Word character (Unicode-aware by default) |
| `\s` | Whitespace |
| `\D`, `\W`, `\S` | Negated forms |
| `(...)` | Capturing group |
| `(?:...)` | Non-capturing group |
| `a|b` | Either pattern a or b |
| `\b` | Word boundary |

For ASCII-only matching, consider `re.ASCII` where appropriate. Regex syntax and Unicode behavior matter for international text.

## 4. `search`, `match`, and `fullmatch`

```python
import re

print(bool(re.search(r"\d+", "room 42")))      # True
print(bool(re.match(r"\d+", "room 42")))       # False
print(bool(re.match(r"\d+", "42 room")))       # True
print(bool(re.fullmatch(r"\d+", "42")))        # True
print(bool(re.fullmatch(r"\d+", "42 room")))   # False
```

Use `fullmatch()` when validating that the entire input follows a pattern. For example, a simple numeric identifier:

```python
if re.fullmatch(r"[0-9]{4}", user_code):
    print("Four ASCII digits")
```

Regex format checks do not prove that an email address or other value is valid in every real-world sense.

## 5. Groups and match objects

```python
import re

match = re.search(r"(?P<user>\w+)@(?P<domain>[\w.-]+)", "a@example.com")
if match:
    print(match.group(0))       # full match
    print(match.group(1))       # first capturing group
    print(match.group("user"))  # named group
    print(match.groupdict())    # dictionary of named groups
    print(match.start(), match.end(), match.span())
```

- `group(0)` is the full match.
- `group(1)` is the first captured group.
- `groups()` returns captured groups as a tuple.
- `groupdict()` returns named groups as a dictionary.

## 6. Find all matches

```python
import re

text = "IDs: 123, 456, and 789"
print(re.findall(r"\d+", text))  # ['123', '456', '789']
```

If the pattern has capturing groups, `findall()` may return strings or tuples of captured groups rather than full-match strings. Use `finditer()` when you need match positions or more predictable access to match objects.

## 7. Replace and split

```python
import re

text = "one   two\tthree"
print(re.sub(r"\s+", " ", text))       # one two three
print(re.split(r"[,;]\s*", "red, green;blue"))
```

Use a function as the replacement when replacement depends on the match:

```python
result = re.sub(r"\d+", lambda m: str(int(m.group()) * 2), "A3 B4")
print(result)  # A6 B8
```

## 8. Flags

Common flags:
- `re.IGNORECASE` / `re.I`: case-insensitive matching.
- `re.MULTILINE` / `re.M`: `^` and `$` apply to line boundaries.
- `re.DOTALL` / `re.S`: `.` also matches newline.
- `re.VERBOSE` / `re.X`: allow whitespace and comments in patterns.
- `re.ASCII` / `re.A`: make selected character classes ASCII-oriented.

```python
pattern = re.compile(r"""
    ^              # start
    (?P<code>[A-Z]{2})
    -
    (?P<number>\d{4})
    $              # end
""", re.VERBOSE)

match = pattern.fullmatch("AB-1234")
```

## 9. Greedy and non-greedy matching

Quantifiers such as `*` and `+` are greedy by default: they take as much text as they can while allowing the overall pattern to match.

```python
import re

text = "<b>one</b><b>two</b>"
print(re.findall(r"<b>.*</b>", text))   # one large match
print(re.findall(r"<b>.*?</b>", text))  # each tag pair
```

For parsing HTML, prefer an HTML parser such as BeautifulSoup instead of trying to parse arbitrary HTML with regex.

## 10. Security and best practices

- Use `fullmatch()` for whole-input format checks.
- Do not assume a regex alone can validate complex formats perfectly.
- Avoid extremely complex patterns on untrusted input; some patterns can cause catastrophic backtracking (ReDoS).
- Prefer straightforward patterns and test worst-case inputs.
- Use `re.escape()` when inserting user-provided literal text into a pattern.
- Do not use regex as a replacement for parameterized SQL queries or proper HTML sanitization.
- Compile patterns reused many times for clarity and efficiency.

## 11. Practice
1. Extract all numbers from a sentence.
2. Validate a four-digit code with `fullmatch()`.
3. Replace repeated whitespace with one space.
4. Extract username and domain into named groups.
5. Compare greedy and non-greedy matching.
6. Safely search for a literal user-supplied string with `re.escape()`.

**Key idea:** regex describes text patterns; choose the right function for searching, extracting, replacing, or validating.
