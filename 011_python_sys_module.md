# Python `sys` Module — Interpreter and Runtime Information

## 1. What is `sys`?
The standard-library `sys` module provides access to interpreter details and facilities used by the running Python process.

```python
import sys

print(sys.version)
print(sys.executable)
print(sys.platform)
```

## 2. Command-line arguments: `sys.argv`

`sys.argv` is a list of strings passed to a script. The first item is normally the script path/name.

```python
# save as show_args.py
import sys

print(sys.argv)
```

Run:
```bash
python show_args.py hello 123
```
Typical structure: `['show_args.py', 'hello', '123']`. Values are strings; use `argparse` for a user-friendly CLI with validation and help.

## 3. Exit status: `sys.exit()`

```python
import sys

if len(sys.argv) < 2:
    print("Usage: python app.py NAME")
    sys.exit(2)

print("Argument:", sys.argv[1])
sys.exit(0)
```

Common conventions:
- `0`: success.
- Non-zero: some kind of failure; exact meanings depend on the program.
- `sys.exit(code)` raises `SystemExit`. The environment running the script interprets its exit status.

Do not use `sys.exit()` inside reusable library functions unless exiting the whole program is truly intended. Return a value or raise an appropriate exception instead.

## 4. Standard streams

- `sys.stdin`: standard input.
- `sys.stdout`: normal output.
- `sys.stderr`: error/diagnostic output.

```python
import sys

print("Normal message", file=sys.stdout)
print("Warning: check configuration", file=sys.stderr)
```

This distinction helps command-line tools send results and errors separately.

## 5. Python executable and import paths

```python
import sys

print(sys.executable)  # interpreter executable path
print(sys.path)        # places Python searches for imports
```

`sys.path` can help diagnose `ModuleNotFoundError`. Prefer fixing your environment/package layout over permanently modifying import paths without understanding the consequences.

## 6. Exceptions: `sys.exc_info()` and `sys.exception()`

```python
import sys

try:
    int("not a number")
except ValueError:
    print(sys.exception())  # active exception, on supported Python versions
    print(sys.exc_info())   # (exception type, exception value, traceback)
```

Use these inside an exception handler when you need details about the active exception. Often, `except ValueError as error:` is simpler:

```python
try:
    int("abc")
except ValueError as error:
    print(error)
```

`sys.exception()` was added in Python 3.11. `sys.exc_info()` is older and returns a tuple; outside an active handler, its values may be `None`.

## 7. `sys.getsizeof()`

```python
import sys

text = "hello"
print(sys.getsizeof(text))
```

This reports the shallow memory size of an object, not necessarily the total memory used by objects it references.

## 8. Recursion limit

```python
import sys
print(sys.getrecursionlimit())
```

Avoid raising the recursion limit casually: an excessive limit can crash the interpreter. Prefer iterative algorithms when practical.

## 9. `sys.modules`

```python
import sys
import json

print("json" in sys.modules)
```

`sys.modules` is the dictionary of modules already loaded in the current interpreter. It is useful for debugging imports, but manually editing it can create confusing behavior.

## 10. Common mistakes and best practices

- Treating `sys.argv` values as integers without conversion.
- Using `sys.exit()` inside ordinary helper functions.
- Printing errors to stdout when they should go to stderr.
- Assuming every non-zero exit status has a universal meaning.
- Treating `sys.getsizeof()` as total recursive memory use.
- Changing `sys.path` as a quick fix without understanding the environment.

## 11. Practice
1. Print `sys.executable`, `sys.version`, and `sys.platform`.
2. Write a script that accepts a filename through `sys.argv`.
3. Print normal output to stdout and diagnostics to stderr.
4. Handle an exception and inspect it with `sys.exception()`.
5. Explain why `argparse` is often better than manually parsing `sys.argv`.

**Key idea:** `sys` exposes information and controls related to the running Python interpreter and process.
