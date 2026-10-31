# Python Regular Expressions (`re`) — Special Characters and Their Meanings

## 1. What Is a Regular Expression?

A **regular expression (regex)** is a pattern used to search, match, extract, split, or replace text.

Python provides the built-in `re` module.

```python
import re

text = "My phone number is 677123456"

result = re.search(r"\d+", text)

if result:
    print(result.group())

# Output:
# 677123456
```

Explanation:

- `import re` imports Python's regular-expression module.
- `r"\d+"` is the pattern.
- `\d` means a digit.
- `+` means one or more repetitions.
- `re.search()` searches for the first matching part.
- `.group()` returns the matched text.

**Important:** Use raw strings such as `r"\d+"` for most regex patterns in Python. The `r` prevents Python from interpreting backslashes as string escape sequences before the regex engine receives them.

---

# 2. Special Sequences: `\d`, `\w`, `\s`, and Others

A special sequence usually begins with a backslash (`\`).

| Pattern | Meaning | Example | Matches |
|---|---|---|---|
| `\d` | A digit | `r"\d"` | `7` |
| `\D` | A non-digit | `r"\D"` | `A`, `@`, `x` |
| `\w` | A Unicode word character | `r"\w"` | `a`, `Z`, `7`, `_` |
| `\W` | A non-word character | `r"\W"` | `@`, `!`, space |
| `\s` | A whitespace character | `r"\s"` | Space, tab, newline |
| `\S` | A non-whitespace character | `r"\S"` | `A`, `7`, `@` |
| `\b` | A word boundary | `r"\bcat\b"` | The whole word `cat` |
| `\B` | A position that is not a word boundary | `r"\Bcat"` | `cat` when preceded by a word character |
| `\A` | Start of the entire string | `r"\AHello"` | `Hello` at the beginning |
| `\Z` | End of the entire string | `r"end\Z"` | `end` at the end |
| `\\` | A literal backslash | `r"\\"` | `\` |

For Python's default Unicode string patterns, `\w` includes Unicode letters and digits as well as `_`. The `re.ASCII` flag changes the behavior of sequences such as `\w`, `\d`, and `\s` to ASCII-based matching.

## Examples

```python
import re

text = "User: Charles_25, Score: 98!"

print(re.findall(r"\d", text))
# ['2', '5', '9', '8']

print(re.findall(r"\d+", text))
# ['25', '98']

print(re.findall(r"\D+", text))
# ['User: Charles_', ', Score: ']

print(re.findall(r"\w+", text))
# ['User', 'Charles_25', 'Score', '98']

print(re.findall(r"\W+", text))
# [' ', ': ', ', ', ': ', '!']

print(re.findall(r"\s", text))
# Every individual whitespace character

print(re.findall(r"\S+", text))
# ['User:', 'Charles_25,', 'Score:', '98!']
```

### Important difference

- `\d` matches one digit.
- `\d+` matches one or more consecutive digits.
- `\D` matches one non-digit.
- `\D+` matches one or more consecutive non-digits.

---

# 3. Regex Metacharacters

Metacharacters are symbols that have a special meaning in regex patterns.

| Character | Meaning | Example | Explanation |
|---|---|---|---|
| `.` | Any character except newline by default | `r"a.c"` | Matches `abc`, `a7c`, `a-c` |
| `^` | Start of string or line | `r"^Hello"` | Matches `Hello` at the beginning |
| `$` | End of string or line | `r"end$"` | Matches `end` at the end |
| `*` | Zero or more repetitions | `r"ab*"` | Matches `a`, `ab`, `abb` |
| `+` | One or more repetitions | `r"ab+"` | Matches `ab`, `abb`, but not `a` |
| `?` | Zero or one repetition | `r"colou?r"` | Matches `color` and `colour` |
| `{n}` | Exactly `n` repetitions | `r"\d{4}"` | Matches four digits |
| `{n,}` | At least `n` repetitions | `r"\d{2,}"` | Matches two or more digits |
| `{n,m}` | Between `n` and `m` repetitions | `r"\d{2,4}"` | Matches two to four digits |
| `[]` | Character class | `r"[abc]"` | Matches `a`, `b`, or `c` |
| `[^...]` | Negated character class | `r"[^0-9]"` | Matches a character that is not a digit |
| `()` | Capturing group | `r"(ab)+"` | Groups `ab` together |
| `(?:...)` | Non-capturing group | `r"(?:ab)+"` | Groups without capturing the group separately |
| `|` | Alternation (OR) | `r"cat\|dog"` is NOT the normal form | Use `r"cat|dog"` to match either word |
| `\` | Escape or introduce a special sequence | `r"\."` | Matches a literal period |

**Remember:** In a regex pattern, alternation is written as `cat|dog`, without backslashes.

---

# 4. The Dot (`.`)

The dot matches almost any single character except a newline by default.

```python
import re

text = "cat cot cut c9t"

print(re.findall(r"c.t", text))

# Output:
# ['cat', 'cot', 'cut', 'c9t']
```

The pattern `c.t` means:

1. Find `c`.
2. Match any single non-newline character.
3. Find `t`.

To allow the dot to match newlines, use `re.DOTALL`.

```python
text = "hello\nworld"

print(re.findall(r"hello.world", text, re.DOTALL))

# Output:
# ['hello\nworld']
```

---

# 5. Anchors: `^`, `$`, `\A`, and `\Z`

Anchors match positions in text rather than ordinary characters.

| Pattern | Meaning |
|---|---|
| `^` | Start of the string; with `re.MULTILINE`, start of each line |
| `$` | End of the string or position before a final newline; with `re.MULTILINE`, end of each line |
| `\A` | Absolute start of the string |
| `\Z` | Absolute end of the string |

## Example

```python
import re

text = """Hello Charles
Hello Python
Goodbye Charles"""

print(re.findall(r"^Hello", text, re.MULTILINE))

# Output:
# ['Hello', 'Hello']
```

Without `re.MULTILINE`, `^` normally refers to the beginning of the entire string.

---

# 6. Quantifiers: `*`, `+`, `?`, and `{}`

Quantifiers specify how many times a pattern may occur.

| Quantifier | Meaning | Example | Example matches |
|---|---|---|---|
| `*` | Zero or more | `r"ab*"` | `a`, `ab`, `abb` |
| `+` | One or more | `r"ab+"` | `ab`, `abb` |
| `?` | Zero or one | `r"ab?"` | `a`, `ab` |
| `{3}` | Exactly three | `r"\d{3}"` | `123` |
| `{2,}` | At least two | `r"\d{2,}"` | `12`, `12345` |
| `{2,5}` | Between two and five | `r"\d{2,5}"` | `12`, `123`, `12345` |
| `*?` | Zero or more, non-greedy | `r"<.*?>"` | Shortest matches possible |
| `+?` | One or more, non-greedy | `r"a+?"` | One `a` at a time when searching |
| `??` | Zero or one, non-greedy | `r"a??"` | Prefers zero repetitions |
| `{2,5}?` | Non-greedy bounded repetition | `r"\d{2,5}?"` | Prefers the smallest permitted match |

## Greedy versus non-greedy

A greedy quantifier tries to match as much text as possible.

A non-greedy quantifier tries to match as little text as possible while still satisfying the pattern.

```python
import re

text = "<b>Hello</b><b>World</b>"

print(re.findall(r"<.*>", text))
# ['<b>Hello</b><b>World</b>']

print(re.findall(r"<.*?>", text))
# ['<b>', '</b>', '<b>', '</b>']
```

The `?` following `*` changes the quantifier to non-greedy.

---

# 7. Character Classes: `[]`

Square brackets let you specify which individual characters are allowed.

| Pattern | Meaning |
|---|---|
| `[abc]` | Match `a`, `b`, or `c` |
| `[a-z]` | Match a lowercase ASCII letter |
| `[A-Z]` | Match an uppercase ASCII letter |
| `[a-zA-Z]` | Match an ASCII letter |
| `[0-9]` | Match an ASCII digit |
| `[a-zA-Z0-9]` | Match an ASCII letter or digit |
| `[a-zA-Z0-9_]` | Match an ASCII letter, digit, or underscore |
| `[^abc]` | Match any character except `a`, `b`, or `c` |
| `[^0-9]` | Match a character that is not an ASCII digit |
| `[aeiou]` | Match a lowercase vowel |
| `[A-Fa-f0-9]` | Match a hexadecimal digit |

## Examples

```python
import re

text = "Python 3.15 is great!"

print(re.findall(r"[aeiou]", text))
# Lowercase vowels

print(re.findall(r"[A-Z]", text))
# Uppercase ASCII letters

print(re.findall(r"[0-9]", text))
# Individual ASCII digits

print(re.findall(r"[0-9]+", text))
# Consecutive ASCII digits

print(re.findall(r"[^0-9]+", text))
# Consecutive characters that are not ASCII digits
```

**Important:** Inside a character class, most metacharacters lose their special meaning. For example, `.` inside `[.]` represents a literal period.

---

# 8. Word Boundaries: `\b` and `\B`

A word boundary identifies a position between a word character (`\w`) and a non-word character (`\W`), or the beginning or end of a string.

## Example: Match a complete word

```python
import re

text = "cat category scatter cat"

print(re.findall(r"\bcat\b", text))

# Output:
# ['cat', 'cat']
```

It matches `cat` as a complete word, not as part of `category` or `scatter`.

## Compare the patterns

| Pattern | Meaning |
|---|---|
| `cat` | Find `cat` anywhere |
| `\bcat` | Find `cat` at a word boundary on its left |
| `cat\b` | Find `cat` at a word boundary on its right |
| `\bcat\b` | Find `cat` as a complete word |
| `\Bcat` | Find `cat` where its left position is not a word boundary |
| `cat\B` | Find `cat` where its right position is not a word boundary |

**Python string warning:** `\b` means a word boundary in a raw regex pattern. In a normal Python string, `"\b"` represents a backspace character. Prefer `r"\b"` for regex word boundaries.

---

# 9. Groups: `()`, `(?:...)`, and `(?P<name>...)`

Groups let you combine patterns and capture parts of a match.

## 9.1 Capturing group: `(...)`

```python
import re

text = "Date: 2026-10-31"

match = re.search(r"(\d{4})-(\d{2})-(\d{2})", text)

if match:
    print(match.group(0))  # Entire match: 2026-10-31
    print(match.group(1))  # Year: 2026
    print(match.group(2))  # Month: 10
    print(match.group(3))  # Day: 31
```

- `group(0)` returns the entire match.
- `group(1)` returns the first capturing group.
- `group(2)` returns the second capturing group.

## 9.2 Non-capturing group: `(?:...)`

Use this when you need grouping but do not need a separate captured result.

```python
pattern = r"(?:Mr|Mrs|Ms)\.?\s+\w+"
```

This matches titles such as `Mr Charles` and `Mrs. Jane`.

## 9.3 Named group: `(?P<name>...)`

Named groups give captured parts meaningful names.

```python
import re

text = "User: Charles, Age: 25"

pattern = r"User: (?P<username>\w+), Age: (?P<age>\d+)"

match = re.search(pattern, text)

if match:
    print(match.group("username"))
    print(match.group("age"))
    print(match.groupdict())

# Output:
# Charles
# 25
# {'username': 'Charles', 'age': '25'}
```

---

# 10. Alternation: `|` (OR)

The pipe symbol means OR.

```python
import re

text = "I like Python, JavaScript, and Java."

print(re.findall(r"Python|JavaScript|Java", text))

# Output:
# ['Python', 'JavaScript', 'Java']
```

For alternatives that should be grouped together, use parentheses:

```python
pattern = r"(cat|dog)s?"

print(re.findall(pattern, "cat cats dog dogs"))

# Output:
# ['cat', 'cat', 'dog', 'dog']
```

Here:

- `(cat|dog)` means either `cat` or `dog`.
- `s?` means an optional `s`.

Note that `findall()` returns captured groups when the pattern contains capturing groups. Use `(?:cat|dog)s?` if you do not need to capture the alternatives separately.

---

# 11. Lookahead and Lookbehind

Lookarounds let you check surrounding text without including that surrounding text in the actual match.

## 11.1 Positive lookahead: `(?=...)`

The following text must match the condition.

```python
import re

text = "Python3 Python2 Java17"

print(re.findall(r"\w+(?=\d)", text))

# Output:
# ['Python', 'Python', 'Java']
```

The pattern finds word characters immediately before a digit. The digit is checked but not included in the match.

## 11.2 Negative lookahead: `(?!...)`

The following text must not match the condition.

```python
import re

text = "cat dog catalog"

print(re.findall(r"\b\w+\b(?!\d)", text))
```

This checks that the position immediately after the matched word is not followed by a digit. Be aware that the word boundary and lookahead interact; use a more specific pattern when you need to exclude entire words based on their suffix.

A clearer example:

```python
text = "file.txt file.exe file.pdf"

print(re.findall(r"\bfile\b(?!\.exe)", text))

# Finds 'file' when it is not immediately followed by '.exe'
```

## 11.3 Positive lookbehind: `(?<=...)`

The preceding text must match the condition.

```python
import re

text = "Price: $100, €200, $300"

print(re.findall(r"(?<=\$)\d+", text))

# Output:
# ['100', '300']
```

## 11.4 Negative lookbehind: `(?<!...)`

The preceding text must not match the condition.

```python
import re

text = "100 200 $300"

print(re.findall(r"(?<!\$)\b\d+\b", text))

# Output:
# ['100', '200']
```

**Important:** Python's built-in `re` module generally requires lookbehind patterns to have a fixed length. For example, `(?<=\$)` is valid, while a lookbehind containing an unrestricted `+` is not.

---

# 12. Escaping Special Characters

Some characters have special meanings in regex. To match them literally, escape them with a backslash.

| Literal character | Regex pattern | Meaning |
|---|---|---|
| `.` | `r"\."` | Literal period |
| `*` | `r"\*"` | Literal asterisk |
| `+` | `r"\+"` | Literal plus sign |
| `?` | `r"\?"` | Literal question mark |
| `^` | `r"\^"` | Literal caret |
| `$` | `r"\$"` | Literal dollar sign |
| `(` | `r"\("` | Literal opening parenthesis |
| `)` | `r"\)"` | Literal closing parenthesis |
| `[` | `r"\["` | Literal opening square bracket |
| `]` | `r"\]"` | Literal closing square bracket |
| `{` | `r"\{"` | Literal opening curly brace |
| `}` | `r"\}"` | Literal closing curly brace |
| `\` | `r"\\"` | Literal backslash |
| `|` | `r"\|"` | Literal pipe |
| `-` | `r"\-"` | Literal hyphen; especially useful inside character classes |

## Example

```python
import re

text = "Price: $25.50"

match = re.search(r"\$\d+\.\d+", text)

if match:
    print(match.group())

# Output:
# $25.50
```

Explanation:

- `\$` matches a literal dollar sign.
- `\d+` matches one or more digits.
- `\.` matches a literal period.
- `\d+` matches the digits after the period.

---

# 13. Useful Patterns for Real-World Text

## 13.1 Find integers

```python
r"\d+"
```

## 13.2 Find a four-digit year

```python
r"\b\d{4}\b"
```

## 13.3 Find a date in `YYYY-MM-DD` format

```python
r"\b\d{4}-\d{2}-\d{2}\b"
```

This checks the shape of the date, not whether it is a real calendar date.

## 13.4 Find a time in `HH:MM` format

```python
r"\b\d{2}:\d{2}\b"
```

This checks the shape, not whether the time is valid.

## 13.5 Find a simple username

```python
r"\b[A-Za-z_][A-Za-z0-9_]*\b"
```

This allows an ASCII letter or underscore first, followed by ASCII letters, digits, or underscores.

## 13.6 Find a hexadecimal sequence

```python
r"\b[0-9A-Fa-f]+\b"
```

## 13.7 Find a filename ending in `.png`

```python
r"\b[\w-]+\.png\b"
```

## 13.8 Find a simple IPv4-shaped address

```python
r"\b(?:\d{1,3}\.){3}\d{1,3}\b"
```

This checks the shape of an IPv4 address but does **not** ensure each number is between 0 and 255.

## 13.9 Find a line beginning with `ERROR`

```python
r"^ERROR"
```

Use `re.MULTILINE` to check the beginning of every line.

## 13.10 Find a word containing digits

```python
r"\b\w*\d+\w*\b"
```

## 13.11 Find a simple email-shaped string

```python
r"[\w.+-]+@[\w.-]+\.[A-Za-z]{2,}"
```

This is a basic pattern for practice, not a complete email-address validator.

---

# 14. Regex Flags in Python

Flags modify how patterns behave.

| Flag | Meaning |
|---|---|
| `re.IGNORECASE` or `re.I` | Case-insensitive matching |
| `re.MULTILINE` or `re.M` | `^` and `$` work with individual lines |
| `re.DOTALL` or `re.S` | `.` also matches newline characters |
| `re.ASCII` or `re.A` | ASCII-based behavior for sequences such as `\w`, `\d`, and `\s` |
| `re.VERBOSE` or `re.X` | Allows whitespace and comments in patterns, outside character classes |
| `re.DEBUG` | Displays information about how the pattern is compiled |

## Example: Ignore case

```python
import re

text = "Python PYTHON python"

print(re.findall(r"python", text, re.IGNORECASE))

# Output:
# ['Python', 'PYTHON', 'python']
```

## Example: Verbose regex

```python
import re

pattern = re.compile(
    r"""
    \b          # Word boundary
    \d{4}       # Four digits
    -
    \d{2}       # Two digits
    -
    \d{2}       # Two digits
    \b          # Word boundary
    """,
    re.VERBOSE
)

text = "The date is 2026-10-31."

print(pattern.findall(text))

# Output:
# ['2026-10-31']
```

---

# 15. Main Functions in Python's `re` Module

| Function | Meaning |
|---|---|
| `re.search()` | Finds the first match anywhere in a string |
| `re.match()` | Checks for a match at the beginning of a string |
| `re.fullmatch()` | Checks whether the entire string matches |
| `re.findall()` | Returns all non-overlapping matches in a list |
| `re.finditer()` | Returns an iterator of match objects |
| `re.sub()` | Replaces matching text |
| `re.subn()` | Replaces matching text and returns the replacement count |
| `re.split()` | Splits a string using a regex pattern |
| `re.compile()` | Compiles a pattern for reuse |
| `re.escape()` | Escapes characters that have special meanings in a regex pattern |

## Examples

```python
import re

text = "Python 123 Java 456"

# Find the first match
print(re.search(r"\d+", text).group())
# 123

# Find all matches
print(re.findall(r"\d+", text))
# ['123', '456']

# Replace digits
print(re.sub(r"\d+", "NUMBER", text))
# Python NUMBER Java NUMBER

# Split on whitespace
print(re.split(r"\s+", text))
# ['Python', '123', 'Java', '456']

# Match only if the entire string contains digits
print(bool(re.fullmatch(r"\d+", "12345")))
# True

print(bool(re.fullmatch(r"\d+", "123abc")))
# False
```

---

# 16. Common Mistakes to Avoid

## Mistake 1: Forgetting the raw string prefix

Prefer:

```python
pattern = r"\d+"
```

Instead of:

```python
pattern = "\d+"
```

The second form may work for this particular pattern, but raw strings make regex patterns clearer and help avoid Python escape-sequence problems.

## Mistake 2: Confusing `*` and `+`

```python
r"a*"  # Zero or more 'a' characters
r"a+"  # One or more 'a' characters
```

The first can match an empty string. The second requires at least one `a`.

## Mistake 3: Confusing `\d` and `[0-9]`

```python
r"\d+"    # Unicode digits by default
r"[0-9]+" # ASCII digits only
```

Use `[0-9]` when your pattern specifically requires ASCII digits.

## Mistake 4: Using `match()` when you need to search the entire string

```python
re.match(r"\d+", "Age: 25")   # No match at the beginning
re.search(r"\d+", "Age: 25")  # Finds 25
```

## Mistake 5: Forgetting that `findall()` may return captured groups

```python
re.findall(r"(\d+)-([A-Z]+)", "123-ABC")
# [('123', 'ABC')]
```

When a pattern has capturing groups, `findall()` returns the captured groups rather than always returning the entire matched text.

Use non-capturing groups `(?:...)` if you need grouping without separately capturing that group.

## Mistake 6: Assuming a regex validates everything

A regex can check the format of a date, IP address, email, or other value without proving that the value is valid.

For example, a pattern may match `99:99` even though that is not a valid time.

---

# 17. Quick Revision Cheat Sheet

Copy this section for fast reference.

```text
SPECIAL SEQUENCES
\d       Digit
\D       Non-digit
\w       Word character
\W       Non-word character
\s       Whitespace
\S       Non-whitespace
\b       Word boundary
\B       Not a word boundary
\A       Start of entire string
\Z       End of entire string
\\       Literal backslash

METACHARACTERS
.        Any character except newline by default
^        Start of string or line
$        End of string or line
*        Zero or more
+        One or more
?        Zero or one
{n}      Exactly n repetitions
{n,}     At least n repetitions
{n,m}    Between n and m repetitions
[]       Character class
[^...]   Negated character class
()       Capturing group
(?:...)  Non-capturing group
|        OR / alternation
\        Escape or special-sequence prefix

NON-GREEDY QUANTIFIERS
*?       Zero or more, non-greedy
+?       One or more, non-greedy
??       Zero or one, non-greedy
{n,m}?   Bounded repetition, non-greedy

GROUPS
(...)           Capturing group
(?:...)         Non-capturing group
(?P<name>...)   Named capturing group
(?P=name)       Backreference to a named group
\1              Backreference to the first capturing group

LOOKAROUNDS
(?=...)         Positive lookahead
(?!...)         Negative lookahead
(?<=...)        Positive lookbehind
(?<!...)        Negative lookbehind

FLAGS
re.I             Ignore case
re.M             Multiline
re.S             Dot matches newlines
re.A             ASCII behavior for special sequences
re.X             Verbose regex
re.DEBUG         Show compiled pattern information

COMMON FUNCTIONS
re.search()      Find first match anywhere
re.match()       Match at the beginning
re.fullmatch()   Match the entire string
re.findall()     Find all non-overlapping matches
re.finditer()    Iterate over match objects
re.sub()         Replace matches
re.subn()        Replace and count replacements
re.split()       Split using a pattern
re.compile()     Compile a reusable pattern
re.escape()      Escape regex special characters
```

---

# 18. Recommended Learning Order

Practise these in order:

1. `\d`, `\D`, `\w`, `\W`, `\s`, and `\S`.
2. Character classes such as `[A-Z]`, `[0-9]`, and `[^0-9]`.
3. Quantifiers such as `*`, `+`, `?`, and `{n,m}`.
4. Anchors such as `^`, `$`, `\A`, and `\Z`.
5. Word boundaries such as `\b` and `\B`.
6. Groups and alternation: `()`, `(?:...)`, and `|`.
7. Greedy and non-greedy matching.
8. Lookahead and lookbehind.
9. Regex flags.
10. Practical searches involving logs, filenames, dates, usernames, and network data.

**Best practice:** Start with a small string, test one regex feature at a time, and inspect the output. Once you understand each feature independently, combine them into more complex patterns.
