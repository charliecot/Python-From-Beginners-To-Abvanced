# Python `argparse` Module — Command-Line Arguments

## 1. What is `argparse`?
`argparse` is part of Python's standard library. It helps you build command-line interfaces so users can pass options when starting a script instead of editing the code.

```bash
python greet.py --name Charles
```

## 2. First program
Save as `greet.py`:
```python
import argparse

parser = argparse.ArgumentParser(
    description="Greet a person from the command line."
)
parser.add_argument("--name", help="Person's name", default="Guest")

args = parser.parse_args()
print(f"Hello, {args.name}!")
```
Run:
```bash
python greet.py
python greet.py --name Charles
python greet.py --help
```
`parse_args()` returns a namespace; `--name` is accessed as `args.name`.

## 3. Positional vs optional arguments
A positional argument is identified by position:
```python
parser.add_argument("filename", help="File to inspect")
```
Run `python inspect.py report.txt`; access it as `args.filename`.

An optional argument usually begins with `-` or `--`:
```python
parser.add_argument("--output", help="Output directory")
parser.add_argument("-v", "--verbose", action="store_true")
```
`store_true` makes the value `True` when the flag is present and `False` when absent.

## 4. Types, defaults, and choices
```python
parser.add_argument("--count", type=int, default=3)
parser.add_argument(
    "--level",
    choices=["debug", "info", "warning", "error"],
    default="info",
)
```
`type=int` converts `--count 10` into integer `10`, rather than string `"10"`.

Useful options:
- `type=int`, `type=float`: convert values.
- `default=value`: fallback if omitted.
- `required=True`: require an optional argument.
- `choices=[...]`: restrict allowed values.
- `help="..."`: document the option.
- `metavar="NAME"`: customize help display.

Optional arguments are not required unless `required=True`. Positional arguments are required by default.

## 5. Multiple values and repeated options
### `nargs`
```python
parser.add_argument("--files", nargs="+")
```
Run `python tool.py --files a.txt b.txt c.txt`; `args.files` is a list.

Common `nargs` values:
- `?`: zero or one value.
- `*`: zero or more.
- `+`: one or more.
- An integer such as `3`: exactly that many values.

### `action="append"`
```python
parser.add_argument("--include", action="append", default=None)
```
Run:
```bash
python tool.py --include "*.py" --include "*.md"
```
Values are collected in a list. With `default=None`, normalize to an empty list later if needed.

## 6. Counting verbosity
```python
parser.add_argument("-v", "--verbose", action="count", default=0)
```
`-v`, `-vv`, and `-vvv` produce 1, 2, and 3. This can control logging detail.

## 7. Boolean flags
```python
parser.add_argument("--dry-run", action="store_true")
```
If supplied, `args.dry_run` is `True`.

On supported Python versions, `argparse.BooleanOptionalAction` creates both `--color` and `--no-color`:
```python
parser.add_argument(
    "--color",
    action=argparse.BooleanOptionalAction,
    default=True,
)
```
Check your minimum supported Python version before using it.

## 8. Practical example: backup CLI
Save as `backup_cli.py`:
```python
import argparse
from pathlib import Path

def main() -> None:
    parser = argparse.ArgumentParser(
        description="Validate source and destination for a backup."
    )
    parser.add_argument("source", type=Path, help="File or directory to back up")
    parser.add_argument(
        "--destination", "-d",
        type=Path,
        required=True,
        help="Directory where backups should be stored",
    )
    parser.add_argument("--dry-run", action="store_true")
    parser.add_argument("-v", "--verbose", action="count", default=0)

    args = parser.parse_args()

    if not args.source.exists():
        parser.error(f"Source does not exist: {args.source}")

    if args.dry_run:
        print(f"DRY RUN: would back up {args.source} to {args.destination}")
    else:
        print(f"Backup requested: {args.source} -> {args.destination}")
        print("Connect the actual copy routine here.")

    if args.verbose:
        print(f"Verbosity level: {args.verbose}")

if __name__ == "__main__":
    main()
```
Run:
```bash
python backup_cli.py "E:/my-files" --destination "E:/backups" --dry-run -vv
```
This validates arguments and displays a plan; it deliberately does not copy files. Before adding copying, plan for existing files, permissions, failures, and backup naming.

## 9. Subcommands
Subcommands allow multiple actions, like `git status` and `git commit`:
```python
import argparse

parser = argparse.ArgumentParser(description="Simple file tool")
subparsers = parser.add_subparsers(dest="command", required=True)

list_parser = subparsers.add_parser("list", help="List files")
list_parser.add_argument("directory")

delete_parser = subparsers.add_parser("delete", help="Delete a file")
delete_parser.add_argument("filename")

args = parser.parse_args()

if args.command == "list":
    print(f"Would list: {args.directory}")
elif args.command == "delete":
    print(f"Would delete: {args.filename}")
```
Run `python filetool.py list ./documents` or `python filetool.py delete old.txt`. For destructive operations, validate paths and consider confirmation or dry-run behavior.

## 10. `if __name__ == "__main__":`
This runs `main()` only when the file is executed directly, not when imported. Keep parsing inside `main()` so tests and other modules can import reusable functions without parsing their arguments.

## 11. Common mistakes
- Forgetting `type=int` and receiving a string.
- Confusing `--output-dir` with `args.output_dir` (hyphens normally become underscores).
- Assuming optional arguments are required by default.
- Using `nargs="+"` without supplying values.
- Performing destructive operations without validation or a dry run.
- Putting all application logic into the parser instead of separate functions.
- Calling `parse_args()` at import time in a module tests need to import.

## 12. Quick reference
| Requirement | Example |
|---|---|
| Create parser | `argparse.ArgumentParser()` |
| Add argument | `parser.add_argument(...)` |
| Parse CLI | `parser.parse_args()` |
| Positional | `add_argument("filename")` |
| Optional | `add_argument("--output")` |
| Short + long option | `add_argument("-o", "--output")` |
| Convert to integer | `type=int` |
| Default | `default=...` |
| Require option | `required=True` |
| Restrict choices | `choices=[...]` |
| Boolean flag | `action="store_true"` |
| Count flags | `action="count"` |
| Multiple values | `nargs="+"` |
| Repeated option | `action="append"` |
| Custom error | `parser.error("message")` |
| Automatic help | `--help` |

## 13. Practice
1. Create a greeting CLI with positional `name`.
2. Add `--age` as an integer and validate its range.
3. Add a `--verbose` flag.
4. Make a file scanner accept a directory and file extensions.
5. Add `scan` and `backup` subcommands.
6. Add `--dry-run` before any operation that modifies files.

**Key idea:** `argparse` turns a Python script into a command-line tool with options, validation, and automatic help.
