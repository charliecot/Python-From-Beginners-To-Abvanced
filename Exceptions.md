# Python Exception Handling: Beginner to Advanced

## Table of Contents

1. What are errors and exceptions?
2. How Python raises exceptions automatically
3. The `try` statement
4. Handling exceptions with `except`
5. Handling different exception types
6. The `else` block
7. The `finally` block
8. Raising exceptions with `raise`
9. Re-raising exceptions
10. Exception chaining with `raise from`
11. Common built-in exception types
12. Creating custom exceptions
13. Using `pass` and `assert`
14. Exception handling in functions
15. Exception handling with SQLite
16. Exception handling with files
17. Exception handling in decorators
18. Logging exceptions
19. Advanced exception handling
20. Best practices
21. Common mistakes
22. Practical exercises
23. Quick reference

---

# 1. What Are Errors and Exceptions?

When writing Python programs, problems can occur.

For example:

```python
number = 10
result = number / 0
```

Python raises:

```text
ZeroDivisionError: division by zero
```

Why?

Because dividing a number by zero is not permitted.

Python uses exceptions to represent problems that occur during program execution.

## 1.1 Errors can occur at different stages

### A. Syntax errors

A syntax error occurs when Python cannot understand the structure of your code.

```python
if 10 > 5
    print("Ten is greater")
```

The colon is missing.

Correct version:

```python
if 10 > 5:
    print("Ten is greater")
```

Python normally detects syntax errors before executing the affected program.

### B. Runtime exceptions

A runtime exception occurs while Python executes your program.

```python
number = 10

print(number / 0)
```

This raises `ZeroDivisionError`.

Another example:

```python
student = {"name": "Charles"}

print(student["age"])
```

This raises `KeyError` because the dictionary has no `"age"` key.

### C. Logical errors

A logical error occurs when your program runs but produces an incorrect result.

```python
price = 100
discount = 20

final_price = price + discount

print(final_price)
```

Output:

```text
120
```

If you intended to subtract the discount, the correct calculation is:

```python
final_price = price - discount
```

Logical errors do not necessarily raise exceptions.

**Remember:**
- Syntax errors concern the structure of your code.
- Runtime exceptions represent problems encountered during execution.
- Logical errors produce incorrect behavior, even when the code executes successfully.

---

# 2. How Python Raises Exceptions Automatically

You do not always need to raise exceptions yourself. Python raises them when an operation cannot be completed.

### Example 1: Division by zero

```python
result = 10 / 0
```

Exception:

```python
ZeroDivisionError
```

### Example 2: Invalid conversion

```python
age = int("hello")
```

Exception:

```python
ValueError
```

Python cannot convert `"hello"` into an integer.

### Example 3: Missing dictionary key

```python
student = {"name": "Charles"}

print(student["age"])
```

Exception:

```python
KeyError
```

### Example 4: Accessing a nonexistent attribute

```python
number = 100

number.walk()
```

Exception:

```python
AttributeError
```

An integer does not have a `walk()` method.

### Example 5: Missing file

```python
with open("missing.txt", "r", encoding="utf-8") as file:
    print(file.read())
```

This normally raises:

```python
FileNotFoundError
```

Python automatically creates and raises an appropriate exception when an operation fails.

---

# 3. The `try` Statement

The `try` statement allows you to execute code that might raise an exception.

Basic structure:

```python
try:
    # Code that might fail.
    ...
except ExceptionType:
    # Handle the exception.
    ...
```

Example:

```python
try:
    result = 10 / 0

except ZeroDivisionError:
    print("You cannot divide by zero.")
```

Output:

```text
You cannot divide by zero.
```

Without exception handling, the exception would normally terminate the current execution path and produce a traceback.

With exception handling, your program can respond to the problem.

## Important

A `try` statement does not prevent an exception from occurring.

Instead, it lets you decide what happens when an exception is raised.

---

# 4. Handling Exceptions with `except`

The `except` block handles an exception raised inside the associated `try` block.

Example:

```python
try:
    age = int(input("Enter your age: "))

except ValueError:
    print("Please enter a valid integer.")
```

If the user enters:

```text
hello
```

Python raises `ValueError`, and the exception handler runs.

If the user enters:

```text
25
```

the conversion succeeds, and the `except` block is skipped.

## 4.1 Catching an exception object

You can use `as` to access the exception object.

```python
try:
    number = int("hello")

except ValueError as error:
    print("An error occurred:", error)
```

Output:

```text
An error occurred: invalid literal for int() with base 10: 'hello'
```

Here:

```python
except ValueError as error:
```

means:

1. Catch a `ValueError`.
2. Make the exception object available through the variable `error`.

You can inspect the exception's message:

```python
print(str(error))
```

You can also inspect its type:

```python
print(type(error))
```

Output:

```text
<class 'ValueError'>
```

The variable assigned through `as` is intended for use inside the handler. Python clears that exception target when the handler exits.

---

# 5. Handling Different Exception Types

A program can encounter several kinds of exceptions.

You can handle each type separately.

```python
try:
    number = int(input("Enter a number: "))

    result = 100 / number

    print(result)

except ValueError:
    print("You must enter an integer.")

except ZeroDivisionError:
    print("The number cannot be zero.")
```

Possible outcomes:

| Input | Result |
|---|---|
| `10` | Prints `10.0` |
| `0` | Prints a zero-division error message |
| `hello` | Prints an invalid-integer message |
| `-5` | Prints `-20.0` |

Notice that the program handles different problems differently.

## 5.1 Catching multiple exception types together

If you want to handle multiple exceptions in the same way, you can use a tuple.

```python
try:
    number = int("hello")

except (ValueError, TypeError) as error:
    print("Invalid input:", error)
```

The syntax is:

```python
except (ExceptionType1, ExceptionType2) as error:
```

Python handles either exception through the same block.

## 5.2 Catching a broad exception

You may encounter this:

```python
try:
    result = 10 / 0

except Exception as error:
    print("Something went wrong:", error)
```

`Exception` is the common base class for most ordinary application exceptions.

It catches many types of exceptions, including:

- `ValueError`
- `TypeError`
- `ZeroDivisionError`
- `KeyError`
- `FileNotFoundError`
- `sqlite3.Error`

However, it does not catch every possible exception. For example, `KeyboardInterrupt` and `SystemExit` inherit directly from `BaseException`, not `Exception`.

**Best practice:** Catch the specific exception types you expect whenever practical.

Avoid using a broad handler to hide unexpected programming mistakes.

---

# 6. The `else` Block

The `else` block runs when the `try` block completes without raising an exception.

Syntax:

```python
try:
    # Risky operation.
    ...

except SomeException:
    # Runs if the expected exception occurs.
    ...

else:
    # Runs if the try block succeeds.
    ...
```

Example:

```python
try:
    number = int("25")

except ValueError:
    print("Invalid number.")

else:
    print("Conversion succeeded:", number)
```

Output:

```text
Conversion succeeded: 25
```

Now change the input:

```python
try:
    number = int("hello")

except ValueError:
    print("Invalid number.")

else:
    print("Conversion succeeded:", number)
```

Output:

```text
Invalid number.
```

The `else` block does not execute because the conversion failed.

## Why use `else`?

It keeps code that depends on a successful operation outside the `try` block.

For example:

```python
try:
    age = int(input("Enter age: "))

except ValueError:
    print("Invalid age.")

else:
    print("Your age is:", age)
```

The conversion is the operation that might fail. The success message belongs in `else`.

This also reduces the chance of accidentally catching an exception raised by unrelated code.

---

# 7. The `finally` Block

The `finally` block runs when execution leaves the associated `try` statement, whether an exception occurred or not.

Syntax:

```python
try:
    # Operation.
    ...

except SomeException:
    # Exception handling.
    ...

finally:
    # Cleanup.
    ...
```

Example:

```python
try:
    number = 10 / 0

except ZeroDivisionError:
    print("Division failed.")

finally:
    print("Execution reached cleanup.")
```

Output:

```text
Division failed.
Execution reached cleanup.
```

If the division succeeds, `finally` still runs.

## 7.1 Why is `finally` useful?

It is particularly useful for cleanup operations, such as releasing resources.

Examples include:
- Closing database connections.
- Releasing locks.
- Cleaning up temporary resources.

For ordinary file handling, Python's `with` statement is usually preferable.

For example:

```python
with open("data.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

Python manages the file's cleanup when the block exits.

For SQLite:

```python
import sqlite3

connection = sqlite3.connect("school.db")

try:
    cursor = connection.cursor()
    cursor.execute("SELECT * FROM students")
    students = cursor.fetchall()

finally:
    connection.close()
```

This closes the connection even if an exception occurs during the query.

**Important:** `finally` is not an absolute guarantee that cleanup will occur under every conceivable circumstance. Abrupt process termination or a system-level failure can prevent it.

---

# 8. Raising Exceptions with `raise`

So far, Python has raised exceptions automatically.

But sometimes you need to detect an invalid condition and raise an exception yourself.

You use the `raise` statement.

Syntax:

```python
raise ExceptionType("Error message")
```

Example:

```python
age = -5

if age < 0:
    raise ValueError("Age cannot be negative.")
```

Output:

```text
ValueError: Age cannot be negative.
```

Why raise the exception?

Because a negative age violates the rules of this example.

Instead of allowing invalid data to continue through the application, you stop the operation and report the problem.

## 8.1 Using `raise` inside a function

```python
def calculate_age(birth_year: int, current_year: int) -> int:
    if birth_year > current_year:
        raise ValueError("Birth year cannot be in the future.")

    return current_year - birth_year


print(calculate_age(2030, 2026))
```

The function raises `ValueError` because the birth year is later than the current year.

You can catch that exception where you call the function:

```python
try:
    age = calculate_age(2030, 2026)

except ValueError as error:
    print("Could not calculate age:", error)
```

Output:

```text
Could not calculate age: Birth year cannot be in the future.
```

The function is responsible for identifying invalid data. The caller decides how to respond.

## 8.2 Why not simply use `print()`?

Consider this function:

```python
def withdraw(balance: float, amount: float) -> float:
    if amount > balance:
        print("Insufficient funds.")

    return balance
```

Problem: the caller cannot reliably distinguish a successful withdrawal from a failed one just by examining the return value.

A better approach is:

```python
def withdraw(balance: float, amount: float) -> float:
    if amount <= 0:
        raise ValueError("Withdrawal must be positive.")

    if amount > balance:
        raise ValueError("Insufficient funds.")

    return balance - amount
```

Now a failure is explicitly represented by an exception.

The calling code can handle it:

```python
try:
    remaining_balance = withdraw(100, 150)

except ValueError as error:
    print(error)

else:
    print("Remaining balance:", remaining_balance)
```

Output:

```text
Insufficient funds.
```

## 8.3 When should you raise an exception?

Raise an exception when an operation cannot be completed according to its contract.

Examples:

```python
raise ValueError("Age cannot be negative.")
```

Use this when a supplied value is inappropriate.

```python
raise TypeError("Expected a string.")
```

Use this when a value has the wrong type, if your function explicitly requires that type.

```python
raise RuntimeError("The application is not initialized.")
```

Use this for an operation that cannot proceed because of an unexpected runtime state.

Choose the exception that best describes the actual problem.

---

# 9. Re-Raising Exceptions

Sometimes you catch an exception because you need to perform cleanup or logging, but you still want the original exception to propagate.

Use:

```python
raise
```

Example:

```python
try:
    number = int("hello")

except ValueError:
    print("Recording the invalid conversion.")

    raise
```

The exception is raised again.

The original `ValueError` continues propagating, so callers can handle it or Python can report the traceback.

## Difference between `raise` and `raise error`

Inside an exception handler:

```python
except ValueError:
    raise
```

re-raises the active exception without explicitly creating a new exception.

By contrast:

```python
except ValueError as error:
    raise error
```

raises the exception object explicitly and can change how the traceback is presented.

**Best practice:** Use bare `raise` when you simply want to propagate the exception you caught.

---

# 10. Exception Chaining with `raise from`

Sometimes a low-level exception occurs, but your application wants to communicate a higher-level problem.

For example, converting an environment variable into an integer may fail.

You can preserve the original exception as the cause of a more meaningful one.

```python
def get_port(value: str) -> int:
    try:
        return int(value)

    except ValueError as error:
        raise ValueError("The server port must be an integer.") from error
```

Test it:

```python
try:
    port = get_port("hello")

except ValueError as error:
    print(error)
```

Output:

```text
The server port must be an integer.
```

The traceback also preserves the original conversion error as the cause.

The expression:

```python
raise NewException("Message") from error
```

means that the new exception explicitly identifies `error` as its cause.

This is called **exception chaining**.

It is useful when translating low-level failures into errors that make sense at the application level.

You can also suppress the displayed exception context when appropriate:

```python
raise RuntimeError("Application initialization failed.") from None
```

Use this selectively because hiding exception context can make debugging harder.

---

# 11. Common Built-in Exception Types

Python provides many built-in exception classes.

You do not need to memorize every one immediately. Learn the common ones and understand when they occur.

## 11.1 Type and value exceptions

### `TypeError`

Raised when an operation receives an inappropriate type.

```python
"10" + 5
```

Python cannot directly concatenate a string and an integer.

### `ValueError`

Raised when an operation receives a value of the correct general type but an inappropriate value.

```python
int("hello")
```

The input is a string, but its contents cannot represent an integer.

### `NameError`

Raised when Python cannot find a name.

```python
print(student_name)
```

If `student_name` has not been defined in the current accessible scope, Python raises `NameError`.

### `UnboundLocalError`

A subclass of `NameError` raised when a local variable is referenced before it has been assigned.

```python
def example():
    print(number)
    number = 10
```

### `AttributeError`

Raised when an object does not have the requested attribute.

```python
number = 10
number.walk()
```

### `KeyError`

Raised when a dictionary key does not exist.

```python
student = {"name": "Charles"}

print(student["age"])
```

### `IndexError`

Raised when a sequence index is outside its valid range.

```python
numbers = [10, 20, 30]

print(numbers[10])
```

### `ZeroDivisionError`

Raised when a numeric division or modulo operation attempts to divide by zero.

```python
10 / 0
```

## 11.2 File and operating-system exceptions

### `FileNotFoundError`

A requested file or directory cannot be found.

```python
open("missing.txt")
```

### `PermissionError`

An operation is denied because the process lacks the necessary permission.

```python
# Example: opening a protected file for reading.
open("protected.txt", "r")
```

Whether this raises an exception depends on the actual filesystem permissions and environment.

### `IsADirectoryError`

An operation expecting a file encounters a directory instead.

### `NotADirectoryError`

An operation expecting a directory encounters something that is not a directory.

### `OSError`

A general exception for operating-system-related failures.

`FileNotFoundError` and `PermissionError` are subclasses of `OSError`.

## 11.3 Programming and execution exceptions

### `ImportError`

Raised when an import cannot be completed.

### `ModuleNotFoundError`

A subclass of `ImportError` raised when Python cannot find a requested module.

### `RuntimeError`

A general runtime exception used when an error does not fit a more specific built-in category.

### `RecursionError`

Raised when the maximum recursion depth is exceeded.

### `StopIteration`

Used to signal the end of an iterator.

### `AssertionError`

Raised when an assertion condition evaluates to false.

### `MemoryError`

Raised when an operation cannot allocate the required memory.

### `TimeoutError`

Used for an operation that exceeds its allowed time.

### `ConnectionError`

A base class for certain connection-related errors.

Its subclasses include:
- `ConnectionRefusedError`
- `ConnectionResetError`
- `ConnectionAbortedError`
- `BrokenPipeError`

### `KeyboardInterrupt`

Usually raised when a user interrupts the program, commonly with Ctrl+C in a terminal.

It inherits directly from `BaseException`, not `Exception`.

### `SystemExit`

Raised by `sys.exit()` to request program termination.

It also inherits directly from `BaseException`.

**Important:** Avoid routinely catching `BaseException`. Doing so can intercept user interrupts and normal program-exit requests.

## 11.4 Database exceptions

For SQLite, import the module:

```python
import sqlite3
```

Common exceptions include:

| Exception | Meaning |
|---|---|
| `sqlite3.Error` | Base class for SQLite errors |
| `sqlite3.IntegrityError` | A database integrity constraint was violated |
| `sqlite3.OperationalError` | An operational problem occurred |
| `sqlite3.ProgrammingError` | A programming-related database error occurred |
| `sqlite3.DataError` | A data-related error occurred |
| `sqlite3.DatabaseError` | A database-related error occurred |

Examples:

```python
sqlite3.IntegrityError
sqlite3.OperationalError
sqlite3.ProgrammingError
```

The exact exception depends on the operation and the underlying database condition.

---

# 12. Creating Custom Exceptions

Sometimes built-in exceptions do not clearly express the problem in your application.

You can create your own exception classes.

A custom exception normally inherits from `Exception` or from one of your existing custom exception classes.

## 12.1 A basic custom exception

```python
class InvalidAgeError(Exception):
    pass
```

Let's break this down.

```python
class InvalidAgeError(Exception):
```

You define a class named `InvalidAgeError`.

It inherits from `Exception`, making it an exception type.

```python
pass
```

This means the class body contains no additional implementation.

The class still works because it inherits the behavior of `Exception`.

Use it:

```python
age = -5

if age < 0:
    raise InvalidAgeError("Age cannot be negative.")
```

Catch it:

```python
try:
    age = -5

    if age < 0:
        raise InvalidAgeError("Age cannot be negative.")

except InvalidAgeError as error:
    print(error)
```

Output:

```text
Age cannot be negative.
```

## 12.2 Creating several custom exceptions

Imagine you are developing a student management system.

```python
class StudentError(Exception):
    """Base exception for student-related problems."""


class StudentNotFoundError(StudentError):
    """Raised when a student does not exist."""


class DuplicateStudentError(StudentError):
    """Raised when a student already exists."""


class InvalidStudentDataError(StudentError):
    """Raised when student information is invalid."""
```

Notice the inheritance structure:

```text
Exception
    |
    StudentError
        |
        +-- StudentNotFoundError
        |
        +-- DuplicateStudentError
        |
        +-- InvalidStudentDataError
```

Because the three specific exceptions inherit from `StudentError`, you can catch them individually:

```python
try:
    raise StudentNotFoundError("Student ID 10 was not found.")

except StudentNotFoundError as error:
    print("Missing student:", error)
```

Or catch all student-related exceptions:

```python
try:
    raise DuplicateStudentError("This student already exists.")

except StudentError as error:
    print("Student operation failed:", error)
```

This design is useful when an application has many related operations.

## 12.3 A custom exception with `__init__`

You can store additional information inside a custom exception.

```python
class InsufficientFundsError(Exception):
    def __init__(self, balance: float, requested: float):
        self.balance = balance
        self.requested = requested

        super().__init__(
            f"Balance {balance} is insufficient for withdrawal {requested}."
        )
```

Use it:

```python
try:
    raise InsufficientFundsError(100, 150)

except InsufficientFundsError as error:
    print(error)
    print("Available balance:", error.balance)
    print("Requested amount:", error.requested)
```

The exception contains both:
- A readable message.
- Structured information that the caller can inspect.

You do not need to override `__init__` for every custom exception. Do so when your exception needs additional data or custom initialization.

---

# 13. Using `pass` and `assert`

These two keywords have different purposes.

## 13.1 The `pass` statement

`pass` is a placeholder that performs no operation.

Example:

```python
class DatabaseError(Exception):
    pass
```

This is useful when defining an exception class that does not need additional behavior.

However, avoid silently ignoring exceptions:

```python
try:
    result = 10 / 0

except ZeroDivisionError:
    pass
```

The code continues without reporting the problem.

That may be appropriate in a narrow situation, but it can also hide a serious bug.

## 13.2 The `assert` statement

`assert` checks a condition and raises `AssertionError` if the condition is false.

```python
age = 25

assert age >= 0
```

No exception is raised because the condition is true.

Now:

```python
age = -5

assert age >= 0, "Age cannot be negative."
```

This raises:

```text
AssertionError: Age cannot be negative.
```

### When should you use `assert`?

Use assertions to express assumptions that should hold during development and testing.

For example:

```python
def calculate_total(prices: list[float]) -> float:
    assert all(price >= 0 for price in prices)

    return sum(prices)
```

However, assertions can be disabled when Python runs with optimization enabled.

**Do not use `assert` to validate untrusted user input or enforce essential security checks.** Use explicit conditions and raise appropriate exceptions instead.

---

# 14. Exception Handling in Functions

A function should have a clear responsibility.

It can:
1. Detect an invalid condition.
2. Raise an appropriate exception.
3. Allow the caller to handle the exception.

Example:

```python
def divide(a: float, b: float) -> float:
    if b == 0:
        raise ZeroDivisionError("The denominator cannot be zero.")

    return a / b
```

The caller handles the exception:

```python
try:
    result = divide(10, 0)

except ZeroDivisionError as error:
    print("Calculation failed:", error)

else:
    print("Result:", result)
```

This separates the calculation from the decision about what to do when it fails.

## Should every function catch its own exceptions?

No.

Consider:

```python
def get_student(student_id: int):
    # Database query happens here.
    ...
```

If the function catches every possible exception and simply prints a message, the caller may not know whether the operation succeeded.

Often, it is better to allow meaningful exceptions to propagate to a layer that can handle them appropriately.

For example:
- A database helper handles a database operation.
- A service layer translates database failures into application-level errors.
- A command-line interface displays useful messages.
- A web API translates failures into suitable HTTP responses.

Not every function needs its own `try`/`except` block.

---

# 15. Exception Handling with SQLite

Since you are working with SQLite, let's apply the concepts to your student database.

## 15.1 Catching database exceptions

```python
import sqlite3

connection = sqlite3.connect("school.db")

try:
    cursor = connection.cursor()

    cursor.execute(
        "SELECT * FROM students WHERE id = ?",
        (1,),
    )

    students = cursor.fetchall()

except sqlite3.Error as error:
    print("Database operation failed:", error)

finally:
    connection.close()
```

Here:

```python
except sqlite3.Error as error:
```

catches exceptions belonging to SQLite's error hierarchy.

This is useful when you want to handle database failures without catching unrelated programming errors.

## 15.2 Handling failed inserts

```python
import sqlite3

connection = sqlite3.connect("school.db")

try:
    cursor = connection.cursor()

    cursor.execute(
        """
        INSERT INTO students (name, email)
        VALUES (?, ?)
        """,
        ("Charles", "charles@example.com"),
    )

    connection.commit()

except sqlite3.IntegrityError as error:
    connection.rollback()
    print("A database constraint was violated:", error)

except sqlite3.Error as error:
    connection.rollback()
    print("Database operation failed:", error)

finally:
    connection.close()
```

The `IntegrityError` handler appears before the general `sqlite3.Error` handler because `IntegrityError` is a subclass of `sqlite3.Error`.

Python checks the handlers in order and uses the first matching handler.

## 15.3 Why use `rollback()`?

A database operation may fail after a transaction has begun.

```python
connection.rollback()
```

rolls back the current transaction, undoing its uncommitted changes.

By contrast:

```python
connection.commit()
```

commits the current transaction.

A successful database operation is not always automatically committed, so you need to understand transaction management.

## 15.4 Using a SQLite connection context manager

SQLite's connection context manager can simplify transaction handling:

```python
import sqlite3

connection = sqlite3.connect("school.db")

try:
    with connection:
        cursor = connection.cursor()

        cursor.execute(
            """
            INSERT INTO students (name, email)
            VALUES (?, ?)
            """,
            ("Alice", "alice@example.com"),
        )

except sqlite3.IntegrityError as error:
    print("Constraint violation:", error)

except sqlite3.Error as error:
    print("Database error:", error)

finally:
    connection.close()
```

The `with connection:` block manages transaction completion: it commits when the block succeeds and rolls back when an exception escapes the block.

**Important:** The connection context manager does not close the connection. That is why the example still uses `finally` to close it.

Also, if you catch an exception *inside* the `with connection:` block and suppress it, the context manager may see a normally completed block and commit other pending changes. When you need the transaction to roll back, let the exception escape the transaction block.

---

# 16. Exception Handling with Files

Files are another common source of exceptions.

Example:

```python
try:
    with open("students.txt", "r", encoding="utf-8") as file:
        content = file.read()

except FileNotFoundError:
    print("The file does not exist.")

except PermissionError:
    print("You do not have permission to read this file.")

except OSError as error:
    print("An operating-system error occurred:", error)

else:
    print(content)
```

Why are the exception handlers ordered this way?

`FileNotFoundError` and `PermissionError` are subclasses of `OSError`.

If you put `except OSError` first, it would also catch those more specific exceptions, making their individual handlers unreachable for those cases.

The `with open(...)` statement also ensures that the file is closed when the block exits.

---

# 17. Exception Handling in Decorators

A decorator can manage common behavior around a function, including logging or handling particular exceptions.

Example:

```python
from functools import wraps


def handle_errors(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        try:
            return func(*args, **kwargs)

        except ValueError as error:
            print(f"{func.__name__} failed: {error}")
            raise

    return wrapper


@handle_errors
def convert_age(value: str) -> int:
    age = int(value)

    if age < 0:
        raise ValueError("Age cannot be negative.")

    return age
```

Test it:

```python
try:
    convert_age("-5")

except ValueError:
    print("The caller handled the invalid age.")
```

Possible output:

```text
convert_age failed: Age cannot be negative.
The caller handled the invalid age.
```

The decorator catches `ValueError`, reports it, and then re-raises the same exception.

Notice:

```python
except ValueError as error:
    print(f"{func.__name__} failed: {error}")
    raise
```

The bare `raise` is important because the decorator is not supposed to hide the failure.

### When is this useful?

A decorator can centralize:
- Logging.
- Timing.
- Authentication checks.
- Common validation.
- Carefully designed exception handling.

However, avoid making a decorator catch every exception and return `None` automatically. That can make a failed operation look successful.

---

# 18. Logging Exceptions

In larger programs, printing errors is often insufficient.

Python's `logging` module lets you record diagnostic information.

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s",
)

try:
    number = int("hello")

except ValueError:
    logging.exception("Could not convert the input to an integer.")
```

`logging.exception()` records an error-level log message and includes traceback information for the active exception.

It is intended to be used inside an exception handler.

Alternatively:

```python
except ValueError as error:
    logging.error("Conversion failed: %s", error)
```

This logs the exception message but does not automatically include the traceback.

Use `logging.exception()` when the traceback will help diagnose the failure.

For security-sensitive applications, be careful not to log passwords, access tokens, encryption keys, or other sensitive data.

---

# 19. Advanced Exception Handling

## 19.1 Exception hierarchy

Python exceptions follow an inheritance hierarchy.

A simplified view is:

```text
BaseException
|
+-- Exception
|   |
|   +-- ArithmeticError
|   |   |
|   |   +-- ZeroDivisionError
|   |
|   +-- LookupError
|   |   |
|   |   +-- IndexError
|   |   +-- KeyError
|   |
|   +-- OSError
|   |   |
|   |   +-- FileNotFoundError
|   |   +-- PermissionError
|   |
|   +-- ValueError
|   +-- TypeError
|   +-- RuntimeError
|
+-- KeyboardInterrupt
+-- SystemExit
+-- GeneratorExit
```

This is a simplified illustration, not the complete hierarchy.

The inheritance relationship determines which handlers can catch an exception.

For example:

```python
try:
    raise FileNotFoundError("Missing file.")

except OSError as error:
    print("Operating-system error:", error)
```

This works because `FileNotFoundError` inherits from `OSError`.

## 19.2 Exception objects have useful attributes

Exception objects can contain information beyond their displayed messages.

For example:

```python
try:
    int("hello")

except ValueError as error:
    print(type(error))
    print(str(error))
    print(error.args)
```

- `type(error)` identifies the exception class.
- `str(error)` returns its string representation.
- `error.args` contains the arguments used to initialize the exception.

The available attributes depend on the exception type.

## 19.3 Exception groups

Modern Python supports `ExceptionGroup` and `except*`, introduced in Python 3.11.

They allow multiple exceptions to be represented and handled together, which can be useful with concurrent operations.

Example:

```python
try:
    raise ExceptionGroup(
        "Several operations failed",
        [
            ValueError("Invalid value."),
            TypeError("Invalid type."),
        ],
    )

except* ValueError as errors:
    print("Value errors:", errors)

except* TypeError as errors:
    print("Type errors:", errors)
```

Each `except*` handler processes the matching exceptions in the group.

You will not need this for every program. First become comfortable with ordinary `try`, `except`, `raise`, `else`, and `finally`.

---

# 20. Best Practices for Exception Handling

## Rule 1: Catch specific exceptions

Prefer:

```python
try:
    age = int(user_input)

except ValueError:
    print("Enter a valid integer.")
```

Instead of:

```python
try:
    age = int(user_input)

except Exception:
    print("Something went wrong.")
```

The specific version is easier to understand and less likely to conceal unexpected bugs.

## Rule 2: Do not silently swallow exceptions

Avoid:

```python
try:
    save_data()

except Exception:
    pass
```

This hides the failure.

Instead, handle the expected problem or log and re-raise unexpected exceptions.

## Rule 3: Raise meaningful exceptions

Prefer:

```python
raise ValueError("Age must be non-negative.")
```

Instead of:

```python
raise Exception("Error.")
```

Specific exception types help callers understand what went wrong.

## Rule 4: Keep `try` blocks focused

Prefer:

```python
try:
    age = int(user_input)

except ValueError:
    print("Invalid age.")

else:
    save_age(age)
```

This makes it clear that the conversion is the operation expected to raise `ValueError`.

If `save_age()` fails, that failure will not be mistaken for an invalid integer.

## Rule 5: Use `finally` or context managers for cleanup

Use context managers for files and other resources that support them.

For SQLite connections, remember that `with connection:` handles transactions but does not close the connection.

## Rule 6: Do not use exceptions for every ordinary decision

If invalid input is expected, you can validate it explicitly.

```python
if not user_input.isdecimal():
    print("Enter a positive whole number.")
else:
    number = int(user_input)
```

Exceptions remain appropriate when an operation fails or when the function's contract requires an exception for invalid data.

## Rule 7: Do not use `assert` for security validation

Assertions can be disabled under optimization. Use explicit checks for input validation and security rules.

## Rule 8: Avoid catching `BaseException` routinely

Catching `BaseException` can intercept `KeyboardInterrupt`, `SystemExit`, and other exceptions that ordinary application code generally should not suppress.

## Rule 9: Do not expose sensitive information

For web applications and security tools, avoid sending internal tracebacks, database credentials, tokens, or encryption keys to users.

Log appropriate diagnostic details securely and return a safe error message to the user.

## Rule 10: Do not confuse logging with handling

This code logs an error but still re-raises it:

```python
try:
    risky_operation()

except ValueError:
    logging.exception("The operation failed.")
    raise
```

The exception remains active after the handler.

By contrast:

```python
try:
    risky_operation()

except ValueError as error:
    logging.error("The operation failed: %s", error)
```

suppresses that `ValueError` unless some other exception occurs inside the handler.

Choose deliberately whether an exception should be handled, translated, or allowed to propagate.

---

# 21. Common Mistakes

## Mistake 1: Using the wrong exception type

Incorrect:

```python
try:
    int("hello")

except ZeroDivisionError:
    print("Invalid input.")
```

The operation raises `ValueError`, not `ZeroDivisionError`.

Correct:

```python
try:
    int("hello")

except ValueError:
    print("Invalid input.")
```

## Mistake 2: Putting a general handler before a specific one

Incorrect:

```python
try:
    open("missing.txt")

except OSError:
    print("Operating-system error.")

except FileNotFoundError:
    print("File not found.")
```

The `FileNotFoundError` handler is unreachable for that exception because the earlier `OSError` handler catches it.

Correct:

```python
try:
    open("missing.txt")

except FileNotFoundError:
    print("File not found.")

except OSError:
    print("Another operating-system error occurred.")
```

## Mistake 3: Forgetting to raise the exception

Incorrect:

```python
def validate_age(age: int) -> None:
    if age < 0:
        ValueError("Age cannot be negative.")
```

This creates an exception object but does not raise it.

Correct:

```python
def validate_age(age: int) -> None:
    if age < 0:
        raise ValueError("Age cannot be negative.")
```

The keyword `raise` makes the difference.

## Mistake 4: Returning success after failure

Incorrect:

```python
def divide(a, b):
    try:
        return a / b

    except ZeroDivisionError:
        print("Cannot divide by zero.")
        return 0
```

Returning zero can be misleading because zero may be a legitimate calculation result.

Better, when callers need to distinguish failure from success:

```python
def divide(a, b):
    try:
        return a / b

    except ZeroDivisionError:
        raise
```

Or simply allow the exception to propagate:

```python
def divide(a, b):
    return a / b
```

You only need to catch the exception when you can handle it meaningfully.

---

# 22. Practical Exercises

Try these exercises before looking up solutions.

## Exercise 1: Integer conversion

Write a function that accepts a string and returns an integer.

If conversion fails, allow `ValueError` to propagate.

Expected function signature:

```python
def convert_number(value: str) -> int:
    ...
```

Test it with:

```python
convert_number("100")
convert_number("hello")
```

## Exercise 2: Age validation

Write a function that raises `ValueError` when the supplied age is negative.

Test:

```python
validate_age(20)
validate_age(-5)
```

## Exercise 3: Multiple exceptions

Write a program that:

1. Requests two numbers.
2. Converts them to integers.
3. Divides the first by the second.
4. Handles invalid integers.
5. Handles division by zero.
6. Uses `else` to display the successful result.

## Exercise 4: File handling

Write a program that reads `students.txt`.

Handle:
- `FileNotFoundError`
- `PermissionError`
- Other `OSError` exceptions.

Use `with open(...)`.

## Exercise 5: Custom exception

Create:

```python
class StudentNotFoundError(Exception):
    pass
```

Write a function that searches for a student by ID and raises `StudentNotFoundError` when no matching student exists.

## Exercise 6: SQLite error handling

Write a function that inserts a student into SQLite.

Requirements:

1. Open a connection.
2. Execute a parameterized `INSERT` statement.
3. Commit the transaction on success.
4. Roll back on database failure.
5. Close the connection.
6. Raise the exception again after rollback.

## Exercise 7: Exception decorator

Write a decorator that:

1. Calls the decorated function.
2. Catches `ValueError`.
3. Logs the error.
4. Re-raises the exception.
5. Preserves the original function's metadata using `functools.wraps`.

---

# 23. Quick Reference

| Syntax | Purpose |
|---|---|
| `try:` | Runs code that might raise an exception |
| `except ValueError:` | Handles a particular exception type |
| `except ValueError as error:` | Handles the exception and binds its object to `error` |
| `except (ValueError, TypeError):` | Handles either of two exception types |
| `except Exception:` | Handles most ordinary application exceptions |
| `else:` | Runs when the `try` block succeeds without an exception |
| `finally:` | Runs when execution leaves the `try` statement, subject to process termination and other exceptional circumstances |
| `raise ValueError("Message")` | Raises a new exception |
| `raise` | Re-raises the active exception |
| `raise NewError("Message") from error` | Raises a new exception with an explicit cause |
| `class MyError(Exception):` | Defines a custom exception |
| `pass` | Does nothing; often used as a placeholder |
| `assert condition` | Raises `AssertionError` if the condition is false |
| `logging.exception("Message")` | Logs an error message and the active exception traceback |

---

# Final Mental Model

Think of exception handling as a process involving three responsibilities.

**1. Detect the problem.**

Python may raise an exception automatically:

```python
number = int("hello")
```

Or your program may detect a problem and raise one:

```python
if age < 0:
    raise ValueError("Age cannot be negative.")
```

**2. Decide who should handle it.**

A function may catch the exception if it can resolve the problem. Otherwise, it can allow the exception to propagate to its caller.

```python
try:
    perform_operation()

except ValueError as error:
    handle_invalid_input(error)
```

**3. Clean up resources and preserve useful information.**

Use context managers or `finally` for cleanup. Use logging when diagnostic information is needed. Re-raise exceptions when the current layer cannot fully handle the failure.

```python
try:
    perform_operation()

except ValueError:
    logging.exception("Operation failed.")
    raise

finally:
    cleanup()
```

The central principle is:

**Raise an exception when an operation cannot proceed correctly, catch it when you can handle the problem meaningfully, and otherwise let it propagate.**