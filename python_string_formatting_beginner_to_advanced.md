# Python String Formatting: Beginner to Advanced

A practical reference guide covering `f-strings`, `str.format()`, old-style `%` formatting, format specifiers, alignment, padding, number bases, dates, bytes, and cybersecurity examples.

---

## Table of Contents

1. What is string formatting?
2. The three main formatting styles
3. F-strings from basic to advanced
4. Understanding format specifiers
5. Width, alignment, and padding
6. Number formatting
7. Binary, octal, decimal, and hexadecimal
8. Precision and floating-point numbers
9. Formatting strings
10. Formatting lists, tuples, and dictionaries
11. `str.format()` in detail
12. Old-style `%` formatting
13. Formatting dates and times
14. Formatting bytes and byte sequences
15. Advanced f-string features
16. Practical cybersecurity examples
17. Common errors and troubleshooting
18. Best practices
19. Quick reference cheat sheet
20. Practice exercises

---

# 1. What Is String Formatting?

String formatting means placing values inside a string in a controlled, readable way.

For example:

```python
name = "Charles"
age = 25

message = f"My name is {name} and I am {age} years old."

print(message)
```

Output:

```text
My name is Charles and I am 25 years old.
```

Without formatting, you might concatenate strings:

```python
message = "My name is " + name + " and I am " + str(age)
```

Formatting is usually cleaner and easier to maintain.

**Important:** Formatting changes how a value is represented as text. It does not necessarily change the original value or its data type.

```python
number = 15

formatted = f"{number:04d}"

print(formatted)       # 0015
print(type(number))    # <class 'int'>
print(type(formatted)) # <class 'str'>
```

---

# 2. The Three Main Formatting Styles

Python has three commonly encountered string-formatting styles.

| Style | Example | Recommendation |
|---|---|---|
| f-string | `f"Hello {name}"` | Preferred for most new code |
| `str.format()` | `"Hello {}".format(name)` | Useful in some reusable templates |
| `%` formatting | `"Hello %s" % name` | Older style; understand it when reading existing code |

## 2.1 F-strings

An f-string begins with `f` or `F` before the opening quote.

```python
name = "Charles"
age = 25

print(f"{name} is {age} years old.")
```

## 2.2 `str.format()`

```python
name = "Charles"
age = 25

print("{} is {} years old.".format(name, age))
```

## 2.3 Old-style `%` formatting

```python
name = "Charles"
age = 25

print("%s is %d years old." % (name, age))
```

For new Python code, normally choose f-strings.

---

# 3. F-Strings: Basic to Advanced

## 3.1 Insert a variable

```python
username = "nzegge"
ip_address = "192.168.1.10"

print(f"Username: {username}")
print(f"IP address: {ip_address}")
```

## 3.2 Insert expressions

You can place expressions inside `{}`.

```python
first = 10
second = 5

print(f"Total: {first + second}")
print(f"Difference: {first - second}")
print(f"Product: {first * second}")
```

## 3.3 Call functions

```python
name = "charles"

print(f"Username: {name.upper()}")
print(f"Lowercase: {name.lower()}")
print(f"Length: {len(name)}")
```

## 3.4 Use indexing

```python
mac = "A4:5B:7C:2D:9E:F1"

print(f"First character: {mac[0]}")
print(f"First byte: {mac[:2]}")
print(f"Last byte: {mac[-2:]}")
```

## 3.5 Use dictionary values

```python
device = {
    "name": "Computer A",
    "ip": "192.168.1.10",
    "mac": "A4:5B:7C:2D:9E:F1"
}

print(f"Device: {device['name']}")
print(f"IP: {device['ip']}")
print(f"MAC: {device['mac']}")
```

## 3.6 Use a conditional expression

```python
status_code = 200

message = f"Status: {'Success' if status_code == 200 else 'Failed'}"

print(message)
```

## 3.7 Escape curly braces

Normally, `{}` tells Python that you want to insert or format an expression.

To display literal curly braces, double them:

```python
name = "Charles"

print(f"{{name}}")
print(f"{{ {name} }}")
```

Output:

```text
{name}
{ Charles }
```

---

# 4. Understanding Format Specifiers

This is the most important section.

Consider:

```python
number = 15

print(f"{number:04d}")
```

Output:

```text
0015
```

The colon `:` introduces a **format specification**.

General structure:

```text
{value:[fill][align][sign][#][0][width][grouping][.precision][type]}
```

Not every part is required. The order matters.

A simpler way to understand it:

```python
f"{value:format_specifier}"
```

Examples:

```python
number = 15

print(f"{number:d}")   # 15
print(f"{number:04d}") # 0015
print(f"{number:>6d}") #     15
print(f"{number:<6d}") # 15
print(f"{number:^6d}") #   15
```

## 4.1 What does `:02X` mean?

This is useful when generating MAC addresses.

```python
byte = 10

print(f"{byte:02X}")
```

Output:

```text
0A
```

Breakdown:

| Part | Meaning |
|---|---|
| `:` | Start format specification |
| `0` | Use zero as the padding character |
| `2` | Minimum field width of two characters |
| `X` | Uppercase hexadecimal |

Compare:

```python
number = 10

print(f"{number:X}")    # A
print(f"{number:2X}")   #  A
print(f"{number:02X}")  # 0A
print(f"{number:04X}")  # 000A
```

Notice that `2` is a minimum width, not a maximum width.

```python
number = 255

print(f"{number:02X}")  # FF
print(f"{number:04X}")  # 00FF
```

---

# 5. Width, Alignment, and Padding

Width determines the minimum number of characters reserved for a formatted value.

Alignment determines where the value appears within that space.

| Symbol | Meaning |
|---|---|
| `<` | Left alignment |
| `>` | Right alignment |
| `^` | Center alignment |
| `=` | Padding after the sign but before digits (numeric formats) |

## 5.1 Left alignment

```python
name = "Python"

print(f"{name:<12}")
```

Output:

```text
Python      
```

## 5.2 Right alignment

```python
name = "Python"

print(f"{name:>12}")
```

Output:

```text
      Python
```

## 5.3 Center alignment

```python
name = "Python"

print(f"{name:^12}")
```

Output:

```text
   Python   
```

## 5.4 Custom padding

Place the padding character before the alignment symbol.

```python
name = "Python"

print(f"{name:*<12}")
print(f"{name:*>12}")
print(f"{name:*^12}")
```

Output:

```text
Python******
******Python
***Python***
```

## 5.5 Numeric zero padding

```python
number = 42

print(f"{number:05d}")  # 00042
print(f"{number:08d}")  # 00000042
```

## 5.6 The `=` alignment option

It places padding after a numeric sign.

```python
number = -42

print(f"{number:0=6d}")
```

Output:

```text
-00042
```

Compare:

```python
number = -42

print(f"{number:06d}")  # -00042
print(f"{number:*>6d}") # ****-42
print(f"{number:0=6d}") # -00042
```

---

# 6. Number Formatting

Python supports several numeric presentation types.

| Type | Meaning | Example for 255 |
|---|---|---|
| `d` | Decimal integer | `255` |
| `b` | Binary | `11111111` |
| `o` | Octal | `377` |
| `x` | Lowercase hexadecimal | `ff` |
| `X` | Uppercase hexadecimal | `FF` |
| `n` | Locale-aware number | Depends on locale |
| `c` | Unicode character for integer code point | Depends on value |

Example:

```python
number = 255

print(f"Decimal: {number:d}")
print(f"Binary: {number:b}")
print(f"Octal: {number:o}")
print(f"Hex lowercase: {number:x}")
print(f"Hex uppercase: {number:X}")
```

Output:

```text
Decimal: 255
Binary: 11111111
Octal: 377
Hex lowercase: ff
Hex uppercase: FF
```

## 6.1 Show numeric base prefixes

Use `#` to include the base prefix where supported.

```python
number = 255

print(f"{number:#b}") # 0b11111111
print(f"{number:#o}") # 0o377
print(f"{number:#x}") # 0xff
print(f"{number:#X}") # 0XFF
```

## 6.2 Group large numbers

Use commas:

```python
number = 1000000000

print(f"{number:,}")
```

Output:

```text
1,000,000,000
```

Use underscores:

```python
number = 1000000000

print(f"{number:_}")
```

Output:

```text
1_000_000_000
```

Grouping is also useful with binary and hexadecimal:

```python
number = 65535

print(f"{number:_b}")
print(f"{number:_x}")
```

---

# 7. Binary, Octal, Decimal, and Hexadecimal

These number systems are important in networking and cybersecurity.

A single byte has 8 bits.

```python
number = 10

print(f"Decimal: {number}")
print(f"Binary: {number:08b}")
print(f"Hexadecimal: {number:02X}")
print(f"Octal: {number:o}")
```

Output:

```text
Decimal: 10
Binary: 00001010
Hexadecimal: 0A
Octal: 12
```

Why `08b`?

- `0`: Pad with zeros.
- `8`: Minimum width of eight characters.
- `b`: Binary representation.

Why `02X`?

- `0`: Pad with zeros.
- `2`: Minimum width of two characters.
- `X`: Uppercase hexadecimal.

## 7.1 Convert between bases

Formatting displays a number in another base:

```python
number = 255

print(f"{number:b}") # 11111111
print(f"{number:x}") # ff
```

Parsing text converts it into an integer:

```python
binary_value = int("11111111", 2)
hex_value = int("FF", 16)
octal_value = int("377", 8)

print(binary_value) # 255
print(hex_value)    # 255
print(octal_value)  # 255
```

Remember:

- `format()` and f-strings: integer to formatted text.
- `int(text, base)`: text in a given base to integer.

---

# 8. Precision and Floating-Point Numbers

Precision controls the number of digits after the decimal point for fixed-point formatting.

```python
price = 123.456789

print(f"{price:.2f}") # 123.46
print(f"{price:.3f}") # 123.457
print(f"{price:.0f}") # 123
```

Breakdown:

```text
.2f
│ │ └── Fixed-point presentation
│ └──── Two digits after decimal point
└────── Precision begins
```

## 8.1 Scientific notation

Use `e` or `E`.

```python
number = 1234567.89

print(f"{number:.2e}")
print(f"{number:.2E}")
```

Example output:

```text
1.23e+06
1.23E+06
```

## 8.2 General format

Use `g` or `G` to choose between fixed-point and scientific notation depending on the value and precision.

```python
number = 1234567.89

print(f"{number:.4g}")
print(f"{number:.6g}")
```

## 8.3 Percentages

Use `%` to multiply the value by 100 and display a percent sign.

```python
success_rate = 0.9567

print(f"{success_rate:.2%}")
```

Output:

```text
95.67%
```

Important:

```python
value = 50

print(f"{value}%")   # 50%
print(f"{value:.2%}") # 5000.00%
```

The second example multiplies 50 by 100.

## 8.4 Sign formatting

```python
positive = 25
negative = -25

print(f"{positive:+d}") # +25
print(f"{negative:+d}") # -25
print(f"{positive: d}") #  25
print(f"{negative: d}") # -25
```

---

# 9. Formatting Strings

String format specifications support alignment, width, fill characters, and maximum display length.

```python
text = "Cybersecurity"

print(f"{text:>20}")
print(f"{text:<20}")
print(f"{text:^20}")
```

## 9.1 Limit displayed characters

Use `.precision` with strings to limit the maximum number of characters displayed.

```python
message = "Python programming"

print(f"{message:.6}")  # Python
print(f"{message:.10}") # Python pro
```

Combine width and precision:

```python
message = "Cybersecurity"

print(f"{message:>15.6}")
```

This displays at most six characters, right-aligned in a field of width 15.

---

# 10. Formatting Lists, Tuples, and Dictionaries

F-strings can display containers directly, but the result is their normal string representation.

```python
ports = [22, 80, 443]

print(f"Ports: {ports}")
```

Output:

```text
Ports: [22, 80, 443]
```

## 10.1 Join a list into a string

```python
mac_parts = ["A4", "5B", "7C", "2D", "9E", "F1"]

mac = ":".join(mac_parts)

print(mac)
```

Output:

```text
A4:5B:7C:2D:9E:F1
```

`join()` expects an iterable of strings.

If your values are integers, convert them first:

```python
numbers = [10, 20, 30]

result = ", ".join(str(number) for number in numbers)

print(result)
```

## 10.2 Format dictionary values

```python
device = {
    "hostname": "PC-01",
    "ip": "192.168.1.10",
    "port": 443
}

print(f"Host: {device['hostname']}")
print(f"IP: {device['ip']}")
print(f"Port: {device['port']}")
```

## 10.3 Create aligned tables

```python
devices = [
    ("PC-01", "192.168.1.10", "Online"),
    ("PC-02", "192.168.1.20", "Offline"),
    ("PC-03", "192.168.1.30", "Online"),
]

print(f"{'HOST':<12}{'IP ADDRESS':<20}{'STATUS':<12}")
print("-" * 44)

for host, ip, status in devices:
    print(f"{host:<12}{ip:<20}{status:<12}")
```

---

# 11. `str.format()` in Detail

`str.format()` uses `{}` replacement fields.

## 11.1 Positional arguments

```python
print("My name is {} and I am {} years old.".format("Charles", 25))
```

## 11.2 Numbered positions

```python
print("{0} is learning {1}. {0} likes {1}.".format("Charles", "Python"))
```

Output:

```text
Charles is learning Python. Charles likes Python.
```

## 11.3 Named arguments

```python
print("Name: {name}, Age: {age}".format(name="Charles", age=25))
```

## 11.4 Formatting numbers

```python
number = 255

print("{:08b}".format(number))
print("{:02X}".format(number))
print("{:,}".format(1000000))
```

## 11.5 Access dictionary keys

```python
user = {
    "name": "Charles",
    "age": 25
}

print("Name: {0[name]}, Age: {0[age]}".format(user))
```

## 11.6 Nested format specifications

A replacement field can receive its width from another value.

```python
width = 10
number = 42

print("{:0{width}d}".format(number, width=width))
```

Output:

```text
0000000042
```

---

# 12. Old-Style `%` Formatting

You may encounter this style in older Python code.

| Specifier | Meaning |
|---|---|
| `%s` | String representation |
| `%r` | Representation (`repr`) |
| `%d` | Decimal integer |
| `%i` | Integer |
| `%f` | Floating-point |
| `%.2f` | Two decimal places |
| `%e` | Scientific notation |
| `%x` | Lowercase hexadecimal |
| `%X` | Uppercase hexadecimal |
| `%o` | Octal |
| `%%` | Literal percent sign |

Examples:

```python
name = "Charles"
age = 25
price = 12.567

print("Name: %s" % name)
print("Age: %d" % age)
print("Price: %.2f" % price)
print("Hex: %X" % 255)
```

Multiple values:

```python
name = "Charles"
age = 25

print("Name: %s, Age: %d" % (name, age))
```

Named mapping:

```python
user = {
    "name": "Charles",
    "age": 25
}

print("%(name)s is %(age)d years old." % user)
```

For new code, prefer f-strings unless you have a specific reason to use another style.

---

# 13. Formatting Dates and Times

The `datetime` module provides date and time formatting through `strftime()`.

```python
from datetime import datetime

now = datetime.now()

print(now.strftime("%Y-%m-%d"))
print(now.strftime("%H:%M:%S"))
print(now.strftime("%d/%m/%Y %H:%M:%S"))
```

Common format codes:

| Code | Meaning | Example |
|---|---|---|
| `%Y` | Four-digit year | `2026` |
| `%y` | Two-digit year | `26` |
| `%m` | Month number | `11` |
| `%B` | Full month name | `November` |
| `%b` | Abbreviated month | `Nov` |
| `%d` | Day of month | `03` |
| `%A` | Full weekday | `Tuesday` |
| `%a` | Abbreviated weekday | `Tue` |
| `%H` | Hour, 24-hour clock | `14` |
| `%I` | Hour, 12-hour clock | `02` |
| `%M` | Minute | `30` |
| `%S` | Second | `45` |
| `%f` | Microseconds | `123456` |
| `%p` | AM/PM | `PM` |
| `%j` | Day of year | `307` |
| `%z` | UTC offset | `+0100` |
| `%Z` | Time zone name | Depends on timezone |
| `%c` | Locale date and time | Locale-dependent |
| `%x` | Locale date | Locale-dependent |
| `%X` | Locale time | Locale-dependent |
| `%%` | Literal percent sign | `%` |

Example:

```python
from datetime import datetime, timezone

now = datetime.now(timezone.utc)

print(f"UTC time: {now:%Y-%m-%d %H:%M:%S %Z}")
```

Note: `strftime()` formats a date/time object. It does not itself convert between time zones. Use timezone-aware datetimes and `astimezone()` when conversion is needed.

---

# 14. Formatting Bytes and Byte Sequences

This is especially useful for networking, file analysis, and cryptography.

## 14.1 Bytes to hexadecimal

```python
data = b"\x41\x42\x43"

print(data.hex())
```

Output:

```text
414243
```

Add a separator:

```python
print(data.hex(":"))
```

Output:

```text
41:42:43
```

## 14.2 Format individual bytes

```python
data = b"\x0A\x1B\xFF"

for byte in data:
    print(f"{byte:02X}")
```

Output:

```text
0A
1B
FF
```

Why does this work?

Iterating over a `bytes` object produces integers from 0 to 255. Each integer can therefore be formatted using `02X`.

## 14.3 Show binary bytes

```python
data = b"\x41\x42\x43"

for byte in data:
    print(f"{byte:08b}")
```

## 14.4 Display a byte sequence as a readable hexadecimal string

```python
data = b"\xDE\xAD\xBE\xEF"

hex_data = " ".join(f"{byte:02X}" for byte in data)

print(hex_data)
```

Output:

```text
DE AD BE EF
```

---

# 15. Advanced F-String Features

## 15.1 Debugging with `=`

Python 3.8+ supports the f-string debug syntax.

```python
username = "nzegge"
port = 443

print(f"{username=}")
print(f"{port=}")
```

Output:

```text
username='nzegge'
port=443
```

It displays the expression and its value.

You can combine it with formatting:

```python
number = 255

print(f"{number=:02X}")
```

Output:

```text
number=FF
```

## 15.2 Dynamic width

Use a variable to control the width.

```python
width = 10
name = "Python"

print(f"{name:>{width}}")
```

## 15.3 Dynamic precision

```python
precision = 3
number = 12.345678

print(f"{number:.{precision}f}")
```

Output:

```text
12.346
```

## 15.4 Dynamic fill and alignment

```python
fill = "*"
alignment = "^"
width = 20
name = "Python"

print(f"{name:{fill}{alignment}{width}}")
```

Output:

```text
*******Python*******
```

## 15.5 Nested expressions

```python
numbers = [10, 20, 30]

print(f"Count: {len(numbers)}")
print(f"Maximum: {max(numbers)}")
print(f"Total: {sum(numbers)}")
```

## 15.6 Raw f-strings

A raw f-string combines raw-string behavior with f-string interpolation.

```python
name = "Charles"

path = rf"C:\Users\{name}\Documents"

print(path)
```

Raw strings are useful for Windows paths and regular expressions. Remember that raw strings still have restrictions on ending with an odd number of backslashes.

## 15.7 Multiline f-strings

```python
device = "PC-01"
ip = "192.168.1.10"
status = "Online"

report = f"""
Device: {device}
IP:     {ip}
Status: {status}
"""

print(report)
```

## 15.8 F-strings and literal braces in JSON-like text

```python
username = "Charles"

message = f'{{"username": "{username}", "active": true}}'

print(message)
```

For actual JSON data, prefer the `json` module rather than manually constructing JSON strings.

```python
import json

user = {
    "username": "Charles",
    "active": True
}

print(json.dumps(user, indent=4))
```

---

# 16. Practical Cybersecurity Examples

## 16.1 Generate a random MAC-like address

```python
import secrets

mac_parts = []

for _ in range(6):
    byte = secrets.randbelow(256)
    mac_parts.append(f"{byte:02X}")

mac_address = ":".join(mac_parts)

print(mac_address)
```

This produces six random hexadecimal bytes. It does not guarantee the locally administered unicast bits.

For a locally administered unicast MAC address:

```python
import secrets

first_byte = secrets.randbelow(256)

# Set the second-least-significant bit (U/L) to 1.
# Set the least-significant bit (I/G) to 0.
first_byte = (first_byte & 0b11111100) | 0b00000010

mac_parts = [f"{first_byte:02X}"]

for _ in range(5):
    byte = secrets.randbelow(256)
    mac_parts.append(f"{byte:02X}")

mac_address = ":".join(mac_parts)

print(mac_address)
```

`secrets` is appropriate for security-sensitive randomness. Use `random` for simulations and ordinary non-security randomness.

## 16.2 Format a network packet's bytes

```python
packet = b"\x45\x00\x00\x3C\x1C\x46\x40\x00"

formatted = " ".join(f"{byte:02X}" for byte in packet)

print(formatted)
```

Output:

```text
45 00 00 3C 1C 46 40 00
```

## 16.3 Format a port scan report

```python
ports = [22, 80, 443, 8080]

print(f"{'PORT':<10}{'STATUS':<15}")
print("-" * 25)

for port in ports:
    status = "Open"
    print(f"{port:<10}{status:<15}")
```

## 16.4 Format a hash

```python
import hashlib

message = b"Hello World"

digest = hashlib.sha256(message).hexdigest()

print(f"SHA256: {digest}")
print(f"First 16 characters: {digest[:16]}")
```

`hexdigest()` already returns hexadecimal text. Formatting is useful for organizing or displaying the result.

## 16.5 Format a log message

```python
from datetime import datetime, timezone

level = "WARNING"
event = "Suspicious login attempt"
ip = "192.168.1.50"

timestamp = datetime.now(timezone.utc)

log = (
    f"[{timestamp:%Y-%m-%d %H:%M:%S %Z}] "
    f"{level:<8} | {event} | Source IP: {ip}"
)

print(log)
```

For real applications, Python's `logging` module is generally better than manually formatting every log record.

---

# 17. Common Errors and Troubleshooting

## Error 1: Forgetting to convert integers before `join()`

Incorrect:

```python
numbers = [10, 20, 30]

result = ":".join(numbers)
```

Why? `join()` expects strings, not integers.

Correct:

```python
numbers = [10, 20, 30]

result = ":".join(str(number) for number in numbers)

print(result)
```

## Error 2: Confusing `X` with `x`

```python
number = 255

print(f"{number:x}") # ff
print(f"{number:X}") # FF
```

`x` is lowercase hexadecimal; `X` is uppercase hexadecimal.

## Error 3: Confusing width with precision

```python
number = 12.3456

print(f"{number:10.2f}")
```

- `10` is the minimum total field width.
- `.2` is the number of digits after the decimal point.
- `f` is fixed-point notation.

## Error 4: Forgetting that percentage formatting multiplies by 100

```python
value = 0.5

print(f"{value:.0%}") # 50%
```

## Error 5: Using an unsupported format type

```python
text = "Python"

# This is invalid: d is for decimal integers.
# print(f"{text:d}")
```

Choose a format type suitable for the value.

## Error 6: Forgetting literal braces

Incorrect:

```python
# f"{name}" inserts the value of name.
```

To show the braces themselves:

```python
name = "Charles"

print(f"{{name}}")
```

---

# 18. Best Practices

1. Prefer f-strings for most new Python code.
2. Use descriptive variable names.
3. Use `:02X` for displaying a byte as two uppercase hexadecimal digits.
4. Use `:08b` for displaying a byte as eight binary digits.
5. Remember that width is a minimum, not a maximum.
6. Use `join()` when combining a sequence of strings.
7. Convert non-string values before joining them.
8. Use `datetime` and timezone-aware objects for timestamps.
9. Use `json.dumps()` to serialize actual JSON data.
10. Use `secrets` rather than `random` for security-sensitive random values.
11. Avoid building SQL queries by formatting user input into SQL strings. Use parameterized queries.
12. Remember that formatting creates a textual representation; it does not encrypt, hash, or validate the underlying data.

---

# 19. Quick Reference Cheat Sheet

## F-string basics

```python
f"{name}"             # Insert a variable
f"{10 + 5}"           # Evaluate an expression
f"{name.upper()}"     # Call a method
f"{number=}"          # Debug syntax (Python 3.8+)
f"{{literal braces}}" # Display literal braces
```

## Alignment and padding

```python
f"{value:<10}"   # Left align
f"{value:>10}"   # Right align
f"{value:^10}"   # Center align
f"{value:*<10}"  # Left align, pad with *
f"{value:*>10}"  # Right align, pad with *
f"{value:*^10}"  # Center align, pad with *
f"{number:05d}"  # Zero-pad integer to width 5
f"{number:0=6d}" # Padding after sign
```

## Number formats

```python
f"{number:d}"    # Decimal
f"{number:b}"    # Binary
f"{number:o}"    # Octal
f"{number:x}"    # Lowercase hexadecimal
f"{number:X}"    # Uppercase hexadecimal
f"{number:#b}"   # Binary with 0b prefix
f"{number:#o}"   # Octal with 0o prefix
f"{number:#x}"   # Hexadecimal with 0x prefix
f"{number:,}"    # Thousands separators
f"{number:_}"    # Underscore grouping
```

## Floating-point formats

```python
f"{number:.2f}"  # Two decimal places
f"{number:.3f}"  # Three decimal places
f"{number:.2e}"  # Scientific notation
f"{number:.4g}"  # General format, precision 4
f"{number:.2%}"  # Percentage, two decimal places
f"{number:+.2f}" # Always show sign
```

## Networking and bytes

```python
f"{byte:02X}"    # One byte as two uppercase hex digits
f"{byte:02x}"    # One byte as two lowercase hex digits
f"{byte:08b}"    # One byte as eight binary digits
data.hex()       # Bytes to hexadecimal string
data.hex(":")    # Bytes to colon-separated hexadecimal
":".join(parts) # Join strings with colons
```

## Date and time

```python
f"{now:%Y-%m-%d}"          # Year-month-day
f"{now:%H:%M:%S}"          # 24-hour time
f"{now:%d/%m/%Y %I:%M %p}" # Date and 12-hour time
```

---

# 20. Practice Exercises

Try these without looking at the answers first.

### Exercise 1: Basic f-string

Create variables:

```python
name = "Charles"
language = "Python"
```

Print:

```text
Charles is learning Python.
```

### Exercise 2: Hexadecimal

Format these integers as two-character uppercase hexadecimal values:

```python
numbers = [10, 15, 16, 255]
```

Expected:

```text
0A
0F
10
FF
```

### Exercise 3: Binary

Display the number `10` as an eight-bit binary value.

Expected:

```text
00001010
```

### Exercise 4: MAC address

Given:

```python
parts = ["A4", "5B", "7C", "2D", "9E", "F1"]
```

Use `join()` to produce:

```text
A4:5B:7C:2D:9E:F1
```

### Exercise 5: Alignment

Print the following values in aligned columns:

```python
name = "Python"
version = "3.15"
status = "Installed"
```

### Exercise 6: Precision

Format:

```python
price = 1234.56789
```

Expected:

```text
1234.57
```

### Exercise 7: Dynamic formatting

Use variables to control the width and precision:

```python
width = 12
precision = 3
number = 123.456789
```

### Exercise 8: Bytes

Given:

```python
data = b"\x41\x42\x43\xFF"
```

Display:

```text
41 42 43 FF
```

### Exercise 9: Debug formatting

Use the f-string debug syntax to display both the variable name and its value:

```python
ip_address = "192.168.1.10"
```

### Exercise 10: Log formatting

Create a log line containing:
- A timestamp
- A severity level
- An event description
- An IP address

Use alignment to make the severity level occupy eight characters.

---

## Final Concept to Remember

When you see:

```python
f"{value:02X}"
```

Read it from left to right:

1. `f` — this is an f-string.
2. `{}` — insert an expression.
3. `value` — the value to display.
4. `:` — begin formatting instructions.
5. `0` — pad with zeros.
6. `2` — use a minimum width of two characters.
7. `X` — represent the integer in uppercase hexadecimal.

This is why the following code is useful when working with MAC addresses, packet bytes, and hexadecimal data:

```python
byte = 10

formatted_byte = f"{byte:02X}"

print(formatted_byte)  # 0A
```

**Official documentation:** https://docs.python.org/3/library/string.html#format-specification-mini-language
**F-string documentation:** https://docs.python.org/3/reference/lexical_analysis.html#f-strings
