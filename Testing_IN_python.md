# Python Testing, Unit Testing, and Mocking
## From Beginner to Advanced

**Language:** Python  
**Main testing framework:** `unittest`  
**Mocking library:** `unittest.mock`  
**Additional framework:** `pytest`

---

# Table of Contents

1. Understanding software testing
2. Testing manually versus automatically
3. Unit testing explained
4. The testing pyramid
5. Python's `unittest` module
6. Writing your first unit test
7. Understanding test methods and assertions
8. Naming test files and organizing projects
9. Testing functions with different inputs
10. Testing exceptions
11. `setUp()` and `tearDown()`
12. `setUpClass()` and `tearDownClass()`
13. Testing classes and methods
14. Testing SQLite applications
15. Understanding mocking
16. The `Mock` class
17. `return_value` and `side_effect`
18. Verifying that functions were called
19. The `patch()` function
20. Understanding where to patch
21. Mocking database operations
22. Mocking email functions
23. Mocking APIs and network requests
24. Mocking exceptions
25. Testing decorators
26. Unit tests versus integration tests
27. Introduction to `pytest`
28. Code coverage
29. Test-driven development
30. Common mistakes
31. Best practices
32. Practical exercises
33. Quick reference

---

# 1. Understanding Software Testing

Software testing is the process of checking whether a program behaves as expected.

Imagine you have this function:

```python
def add(a: int, b: int) -> int:
    return a + b
```

You expect:

```python
add(2, 3) == 5
```

You can check it manually:

```python
print(add(2, 3))
```

Output:

```text
5
```

But what happens when your program has 100 functions?

What if you modify one function and accidentally break another?

Manually checking every function becomes time-consuming.

This is where automated testing becomes useful.

An automated test executes code and checks whether the actual result matches the expected result.

For example:

```python
assert add(2, 3) == 5
```

If the result is `5`, the assertion succeeds.

If the result is different, Python raises `AssertionError`.

However, for a complete testing system, Python provides tools such as `unittest` and third-party frameworks such as `pytest`.

## Why testing matters

Testing helps you:

- Detect bugs before deploying an application.
- Verify that functions return the correct results.
- Confirm that exceptions are raised correctly.
- Prevent old features from breaking when code changes.
- Check that database operations work.
- Test APIs and network-related code.
- Verify that security checks reject invalid inputs.
- Make refactoring safer.

**Important:** Passing tests does not prove that a program has no bugs. Tests verify the behaviors you have chosen to test.

---

# 2. Testing Manually Versus Automatically

Suppose you write a function that calculates discounts.

```python
def calculate_discount(price: float, percentage: float) -> float:
    return price * percentage / 100
```

## Manual testing

```python
print(calculate_discount(100, 10))
print(calculate_discount(200, 20))
print(calculate_discount(50, 5))
```

Expected output:

```text
10.0
40.0
2.5
```

Manual testing works, but you must inspect the output yourself.

## Automated testing

```python
assert calculate_discount(100, 10) == 10
assert calculate_discount(200, 20) == 40
assert calculate_discount(50, 5) == 2.5
```

Python automatically checks the conditions.

If an assertion fails, execution raises `AssertionError`.

For example:

```python
assert calculate_discount(100, 10) == 20
```

This fails because the function returns `10.0`, not `20`.

These examples use Python's `assert` statement. Later, you will learn how `unittest` provides more descriptive test assertions and integrates them into a test runner.

---

# 3. What Is Unit Testing?

A unit test checks a small, isolated part of a program.

A unit is often a function, method, or small component.

Consider:

```python
def multiply(a: int, b: int) -> int:
    return a * b


def divide(a: float, b: float) -> float:
    if b == 0:
        raise ZeroDivisionError("Cannot divide by zero.")

    return a / b
```

You can test `multiply()` independently from `divide()`.

```python
def test_multiply():
    assert multiply(3, 4) == 12


def test_divide():
    assert divide(10, 2) == 5
```

Each test focuses on a particular behavior.

A useful unit test should generally be:

- **Focused:** Tests one behavior or closely related group of behaviors.
- **Repeatable:** Produces the same result when its conditions are unchanged.
- **Independent:** Does not rely unnecessarily on another test having run first.
- **Fast:** Avoids unnecessary network, database, and filesystem operations.
- **Readable:** Makes it clear what behavior is being verified.

## Why isolation matters

Imagine testing a function that calculates a total price.

If the function also sends an email, connects to a database, and calls an external API, the test becomes more complicated.

You can isolate the calculation from those dependencies.

Mocking is one technique for doing that.

---

# 4. The Testing Pyramid

Testing can happen at different levels.

```text
                 /\
                /  \
               / UI \
              /------\
             /  API   \
            /Integration\
           /--------------\
          /   Unit Tests   \
         /__________________\
```

This is a conceptual model, not a rule that every project must follow exactly.

## Unit tests

Test individual functions or small components.

Example:

```python
def add(a, b):
    return a + b
```

Check that the function returns the expected result.

## Integration tests

Test whether multiple components work together.

Examples:

- A Python function correctly inserts data into SQLite.
- A Django view communicates with the database.
- An API endpoint correctly authenticates a user.

## End-to-end tests

Test a complete user-facing workflow.

For example:

1. A user registers.
2. The user logs in.
3. The user adds a product to a cart.
4. The user checks out.
5. The system creates an order.

A practical project often uses many fast unit tests, a smaller number of integration tests, and a carefully selected set of end-to-end tests.

---

# 5. Python's `unittest` Module

Python includes a testing framework called `unittest`.

You do not need to install it separately.

Import it with:

```python
import unittest
```

A typical unit test contains three parts:

1. Arrange the required data.
2. Act by calling the function.
3. Assert that the result is correct.

This is commonly called **Arrange–Act–Assert (AAA)**.

For example:

```python
import unittest


def add(a: int, b: int) -> int:
    return a + b


class TestAdd(unittest.TestCase):

    def test_add_positive_numbers(self):
        # Arrange
        a = 10
        b = 5

        # Act
        result = add(a, b)

        # Assert
        self.assertEqual(result, 15)


if __name__ == "__main__":
    unittest.main()
```

Save this as:

```text
test_calculator.py
```

Run it from the terminal:

```bash
python -m unittest -v test_calculator.py
```

Example output:

```text
test_add_positive_numbers ... ok

----------------------------------------------------------------------
Ran 1 test in 0.001s

OK
```

The exact execution time may differ.

## Understanding the code

### `class TestAdd(unittest.TestCase):`

Your test class inherits from `unittest.TestCase`.

This provides methods for checking results and reporting test outcomes.

### `def test_add_positive_numbers(self):`

The method name begins with `test_`.

By default, `unittest` discovers test methods whose names begin with `test`.

### `self.assertEqual(result, 15)`

This checks whether `result` equals `15`.

If the values differ, the test fails.

### `unittest.main()`

This starts the test runner when you execute the file directly.

The `if __name__ == "__main__":` guard prevents the runner from starting automatically when the file is imported as a module.

---

# 6. Understanding Test Assertions

Assertions are methods that check whether something is true about your program.

`unittest.TestCase` provides many assertion methods.

## 6.1 `assertEqual()`

Checks equality.

```python
self.assertEqual(2 + 3, 5)
```

## 6.2 `assertNotEqual()`

Checks inequality.

```python
self.assertNotEqual(2 + 3, 6)
```

## 6.3 `assertTrue()`

Checks whether a value is truthy.

```python
self.assertTrue(10 > 5)
```

## 6.4 `assertFalse()`

Checks whether a value is falsy.

```python
self.assertFalse(10 < 5)
```

## 6.5 `assertIs()`

Checks whether two references point to the same object.

```python
value = None

self.assertIs(value, None)
```

This is different from checking whether two objects have equal values.

## 6.6 `assertIsNone()`

Checks whether a value is `None`.

```python
self.assertIsNone(None)
```

## 6.7 `assertIn()`

Checks whether a value belongs to a collection.

```python
self.assertIn("name", {"name": "Charles"})
```

## 6.8 `assertNotIn()`

Checks that a value is absent.

```python
self.assertNotIn("password", {"name": "Charles"})
```

## 6.9 `assertIsInstance()`

Checks whether an object is an instance of a particular type.

```python
self.assertIsInstance(100, int)
```

## 6.10 `assertGreater()`

Checks whether one value is greater than another.

```python
self.assertGreater(10, 5)
```

## 6.11 `assertLess()`

Checks whether one value is less than another.

```python
self.assertLess(5, 10)
```

## 6.12 `assertAlmostEqual()`

Useful when comparing floating-point results.

```python
self.assertAlmostEqual(0.1 + 0.2, 0.3)
```

Floating-point arithmetic can produce small rounding differences, so exact equality is not always appropriate.

## 6.13 `assertRaises()`

Checks whether an operation raises an expected exception.

```python
def divide(a, b):
    if b == 0:
        raise ZeroDivisionError("Cannot divide by zero.")

    return a / b
```

Test it:

```python
import unittest


class TestDivide(unittest.TestCase):

    def test_division_by_zero(self):
        with self.assertRaises(ZeroDivisionError):
            divide(10, 0)
```

The test passes when `divide(10, 0)` raises `ZeroDivisionError`.

If the function returns normally, the test fails.

### Inspecting the exception

You can capture the exception object:

```python
with self.assertRaises(ValueError) as context:
    int("hello")

self.assertIn("invalid literal", str(context.exception))
```

Here:

- `context` stores information about the captured exception.
- `context.exception` is the exception object.
- `str(context.exception)` gives its message.

## 6.14 `assertRaisesRegex()`

Checks that the expected exception occurs and its message matches a regular expression.

```python
with self.assertRaisesRegex(ValueError, "invalid literal"):
    int("hello")
```

Use it when the error message itself is part of the behavior you need to verify.

Avoid relying on exact messages from external libraries unless the message is intentionally part of your application's contract.

---

# 7. Testing Different Inputs

A good test suite checks more than the normal case.

Consider:

```python
def calculate_discount(price: float, percentage: float) -> float:
    if price < 0:
        raise ValueError("Price cannot be negative.")

    if not 0 <= percentage <= 100:
        raise ValueError("Percentage must be between 0 and 100.")

    return price * percentage / 100
```

Possible test cases include:

| Test case | Input | Expected behavior |
|---|---|---|
| Normal discount | `100, 10` | Returns `10` |
| Zero discount | `100, 0` | Returns `0` |
| Full discount | `100, 100` | Returns `100` |
| Negative price | `-100, 10` | Raises `ValueError` |
| Negative percentage | `100, -5` | Raises `ValueError` |
| Excessive percentage | `100, 150` | Raises `ValueError` |

Implement them:

```python
import unittest


class TestDiscount(unittest.TestCase):

    def test_normal_discount(self):
        self.assertEqual(calculate_discount(100, 10), 10)

    def test_zero_discount(self):
        self.assertEqual(calculate_discount(100, 0), 0)

    def test_full_discount(self):
        self.assertEqual(calculate_discount(100, 100), 100)

    def test_negative_price(self):
        with self.assertRaises(ValueError):
            calculate_discount(-100, 10)

    def test_negative_percentage(self):
        with self.assertRaises(ValueError):
            calculate_discount(100, -5)

    def test_percentage_above_100(self):
        with self.assertRaises(ValueError):
            calculate_discount(100, 150)
```

The key is to test both successful and unsuccessful conditions.

---

# 8. Naming and Organizing Test Files

A simple project might look like this:

```text
calculator_project/
│
├── calculator.py
│
└── tests/
    ├── __init__.py
    └── test_calculator.py
```

Your application code belongs in `calculator.py`.

Your tests belong in `tests/test_calculator.py`.

For example, `calculator.py`:

```python
def add(a: int, b: int) -> int:
    return a + b


def subtract(a: int, b: int) -> int:
    return a - b
```

Your test file:

```python
import unittest

from calculator import add, subtract


class TestCalculator(unittest.TestCase):

    def test_add(self):
        self.assertEqual(add(10, 5), 15)

    def test_subtract(self):
        self.assertEqual(subtract(10, 5), 5)


if __name__ == "__main__":
    unittest.main()
```

Run the tests from the project root:

```bash
python -m unittest discover -s tests -v
```

Here:

- `python -m unittest` runs the testing framework.
- `discover` searches for tests.
- `-s tests` specifies the starting directory.
- `-v` requests verbose output.

By default, test discovery looks for files matching `test*.py`.

You do not necessarily need an `__init__.py` file in every test directory for modern Python test discovery, but package structure and discovery behavior can vary with the way you run tests.

---

# 9. `setUp()` and `tearDown()`

Sometimes several tests need the same initial conditions.

For example, suppose every test needs a calculator object.

Instead of creating it repeatedly, you can use `setUp()`.

```python
import unittest


class Calculator:

    def add(self, a, b):
        return a + b


class TestCalculator(unittest.TestCase):

    def setUp(self):
        self.calculator = Calculator()

    def test_add(self):
        self.assertEqual(self.calculator.add(2, 3), 5)

    def test_add_negative_numbers(self):
        self.assertEqual(self.calculator.add(-2, -3), -5)
```

## What does `setUp()` do?

`setUp()` runs before each test method.

When the test runner executes the two test methods, each test receives its own setup.

This helps keep tests independent.

## What does `tearDown()` do?

`tearDown()` runs after each test method, even when the test fails, subject to normal Python execution limitations.

It is useful for cleanup when a resource has been acquired during setup.

```python
def setUp(self):
    self.resource = acquire_resource()

def tearDown(self):
    release_resource(self.resource)
```

This is a conceptual example: `acquire_resource()` and `release_resource()` would need to be defined for your application.

For resources that support context managers, prefer those when practical.

### Execution order

For each test, the typical order is:

```text
setUp()
    |
    v
test method
    |
    v
tearDown()
```

If `setUp()` fails, the test method will not run, and `tearDown()` is not called for that failed setup.

---

# 10. `setUpClass()` and `tearDownClass()`

Sometimes setup is expensive and should happen once for the entire test class.

Use class-level setup methods.

```python
import unittest


class TestExample(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        print("Runs once before the class's tests.")

    @classmethod
    def tearDownClass(cls):
        print("Runs once after the class's tests.")

    def test_one(self):
        self.assertEqual(1 + 1, 2)

    def test_two(self):
        self.assertEqual(2 + 2, 4)
```

Notice:

```python
@classmethod
def setUpClass(cls):
```

`@classmethod` makes the method receive the class as `cls`.

These methods run once for the class when it contains tests to execute, rather than once for each test method.

### When should you use them?

They can be useful for expensive shared resources, but be careful with shared mutable state.

If one test changes shared data, another test might be affected.

For ordinary unit tests, per-test setup is often safer.

---

# 11. Testing Classes and Methods

Suppose you have a bank account class.

```python
class BankAccount:

    def __init__(self, balance: float = 0):
        self.balance = balance

    def deposit(self, amount: float) -> None:
        if amount <= 0:
            raise ValueError("Deposit must be positive.")

        self.balance += amount

    def withdraw(self, amount: float) -> None:
        if amount <= 0:
            raise ValueError("Withdrawal must be positive.")

        if amount > self.balance:
            raise ValueError("Insufficient funds.")

        self.balance -= amount
```

Test it:

```python
import unittest


class TestBankAccount(unittest.TestCase):

    def setUp(self):
        self.account = BankAccount(100)

    def test_initial_balance(self):
        self.assertEqual(self.account.balance, 100)

    def test_deposit(self):
        self.account.deposit(50)

        self.assertEqual(self.account.balance, 150)

    def test_withdraw(self):
        self.account.withdraw(25)

        self.assertEqual(self.account.balance, 75)

    def test_withdraw_insufficient_funds(self):
        with self.assertRaises(ValueError):
            self.account.withdraw(200)

    def test_negative_deposit(self):
        with self.assertRaises(ValueError):
            self.account.deposit(-10)
```

This verifies the class's public behavior.

It checks both valid operations and invalid operations.

---

# 12. Testing SQLite Applications

Now let's connect testing to your SQLite work.

Suppose you have a function that creates a student table and inserts students.

A useful testing strategy is to create a temporary database for each test rather than modifying your real database.

This is an **integration test** because the code interacts with an actual database.

## Example: Student repository

Create `student_repository.py`:

```python
import sqlite3


def create_student_table(connection: sqlite3.Connection) -> None:
    connection.execute(
        """
        CREATE TABLE IF NOT EXISTS students (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            email TEXT NOT NULL UNIQUE
        )
        """
    )


def create_student(
    connection: sqlite3.Connection,
    name: str,
    email: str,
) -> int:
    cursor = connection.execute(
        """
        INSERT INTO students (name, email)
        VALUES (?, ?)
        """,
        (name, email),
    )

    return cursor.lastrowid


def get_students(connection: sqlite3.Connection) -> list[tuple]:
    cursor = connection.execute(
        "SELECT id, name, email FROM students"
    )

    return cursor.fetchall()
```

Notice that these functions receive an existing connection.

This makes it easier to control the database used during testing.

## Test file

Create `test_student_repository.py`:

```python
import sqlite3
import unittest

from student_repository import (
    create_student_table,
    create_student,
    get_students,
)


class TestStudentRepository(unittest.TestCase):

    def setUp(self):
        self.connection = sqlite3.connect(":memory:")

        create_student_table(self.connection)

    def tearDown(self):
        self.connection.close()

    def test_create_student(self):
        student_id = create_student(
            self.connection,
            "Charles",
            "charles@example.com",
        )

        self.connection.commit()

        self.assertIsInstance(student_id, int)

        students = get_students(self.connection)

        self.assertEqual(len(students), 1)
        self.assertEqual(students[0][1], "Charles")

    def test_duplicate_email_fails(self):
        create_student(
            self.connection,
            "Charles",
            "charles@example.com",
        )

        self.connection.commit()

        with self.assertRaises(sqlite3.IntegrityError):
            create_student(
                self.connection,
                "Alice",
                "charles@example.com",
            )


if __name__ == "__main__":
    unittest.main()
```

Run it:

```bash
python -m unittest -v test_student_repository.py
```

## Understanding `":memory:"`

```python
sqlite3.connect(":memory:")
```

creates an in-memory SQLite database.

It behaves like a database for the lifetime of that connection, but it does not create a normal database file on disk.

This makes it useful for isolated tests.

Each test in the example creates a fresh connection and database, so the tests do not depend on the contents of `school.db`.

**Important:** An in-memory database belongs to its connection. Opening another independent connection with `":memory:"` creates another independent database.

Also, the example commits the successful insert before retrieving the data. The duplicate insert raises an integrity error, and the test verifies that the error occurs.

---

# 13. What Is Mocking?

Mocking means replacing a real dependency with a controlled substitute during a test.

Imagine this function:

```python
def send_welcome_email(email: str) -> None:
    # Connect to an email service and send a real email.
    ...
```

You have a registration function:

```python
def register_user(email: str) -> None:
    # Save the user.
    send_welcome_email(email)
```

You want to test whether `register_user()` calls `send_welcome_email()` correctly.

But you don't want to send a real email every time you run the tests.

You can replace `send_welcome_email()` with a mock.

A mock can:
- Record whether it was called.
- Record the arguments it received.
- Return a value you choose.
- Raise an exception when called.
- Simulate different responses.

Python provides the `unittest.mock` module for this purpose.

Import its tools:

```python
from unittest.mock import Mock, patch, MagicMock
```

You do not need to install this module separately.

## Mocking versus testing a real dependency

Suppose your application calls an external API.

A unit test can mock the API call to verify how your code reacts to a particular response.

An integration test can make a real request against an appropriate test service or controlled environment.

Both approaches are useful, but they answer different questions.

A mocked API test does not prove that the external API is reachable or that your request works against the real service.

---

# 14. The `Mock` Class

The `Mock` class creates a mock object.

```python
from unittest.mock import Mock

send_email = Mock()
```

At this point, `send_email` is a mock object, not a real email-sending function.

You can call it:

```python
send_email("charles@example.com")
```

The mock records that call.

Inspect it:

```python
print(send_email.called)
```

Output:

```text
True
```

Inspect the arguments:

```python
print(send_email.call_args)
```

You can also check the arguments using assertion methods.

```python
send_email.assert_called_once_with("charles@example.com")
```

This assertion passes if the mock was called exactly once with that argument.

It fails if the mock was never called, was called more than once, or received different arguments.

---

# 15. `return_value` and `side_effect`

Two important mock features are `return_value` and `side_effect`.

## 15.1 `return_value`

Use `return_value` to specify what a mock returns when called.

```python
from unittest.mock import Mock

get_user = Mock(return_value={"id": 1, "name": "Charles"})

user = get_user(1)

print(user)
```

Output:

```text
{'id': 1, 'name': 'Charles'}
```

The mock returns the value you configured.

This is useful when testing code that depends on a function's result.

## 15.2 `side_effect` with an exception

You can configure a mock to raise an exception.

```python
from unittest.mock import Mock

connect_database = Mock(
    side_effect=ConnectionError("Database unavailable.")
)

try:
    connect_database()

except ConnectionError as error:
    print(error)
```

Output:

```text
Database unavailable.
```

This lets you test failure handling without deliberately making a real database unavailable.

## 15.3 `side_effect` with a function

You can provide a function that determines the mock's behavior.

```python
from unittest.mock import Mock


def calculate_result(value):
    if value < 0:
        raise ValueError("Value cannot be negative.")

    return value * 2


calculate = Mock(side_effect=calculate_result)

print(calculate(5))
```

Output:

```text
10
```

If you call:

```python
calculate(-5)
```

the mock raises `ValueError`.

## 15.4 `side_effect` with a sequence

A sequence can supply different results on successive calls.

```python
from unittest.mock import Mock

get_status = Mock(
    side_effect=["pending", "processing", "completed"]
)

print(get_status())
print(get_status())
print(get_status())
```

Output:

```text
pending
processing
completed
```

This is useful for testing retry logic or workflows in which a dependency returns different results over time.

After the configured sequence is exhausted, another call normally raises `StopIteration`.

---

# 16. Verifying Mock Calls

Mocks provide assertions for checking how a dependency was used.

Suppose:

```python
from unittest.mock import Mock

send_email = Mock()

send_email("alice@example.com")
send_email("charles@example.com")
```

## 16.1 `assert_called()`

Checks that the mock was called at least once.

```python
send_email.assert_called()
```

## 16.2 `assert_called_once()`

Checks that the mock was called exactly once.

```python
send_email.assert_called_once()
```

This would fail for the example above because there were two calls.

## 16.3 `assert_called_with()`

Checks the arguments of the most recent call.

```python
send_email.assert_called_with("charles@example.com")
```

This passes because the last call used that address.

It does not prove that there was only one call.

## 16.4 `assert_called_once_with()`

Checks both the number of calls and the arguments.

```python
send_email = Mock()

send_email("alice@example.com")

send_email.assert_called_once_with("alice@example.com")
```

## 16.5 `assert_not_called()`

Checks that the mock was never called.

```python
send_email = Mock()

send_email.assert_not_called()
```

## 16.6 `call_args_list`

You can inspect all calls.

```python
from unittest.mock import Mock

send_email = Mock()

send_email("alice@example.com")
send_email("charles@example.com")

print(send_email.call_args_list)
```

This is useful when checking the sequence or arguments of several calls.

**Important:** A mock assertion verifies the interaction with the mock. It does not prove that a real email was delivered or that a real database operation succeeded.

---

# 17. The `patch()` Function

The `patch()` function temporarily replaces an object with another object during a test.

It is one of the most important tools in `unittest.mock`.

For example, imagine `registration.py` contains:

```python
def send_welcome_email(email: str) -> None:
    print(f"Sending an email to {email}")


def register_user(email: str) -> str:
    send_welcome_email(email)

    return "registered"
```

You want to test `register_user()` without calling the real email function.

Create `test_registration.py`:

```python
import unittest
from unittest.mock import patch

from registration import register_user


class TestRegistration(unittest.TestCase):

    @patch("registration.send_welcome_email")
    def test_register_user(self, mock_send_email):
        result = register_user("charles@example.com")

        self.assertEqual(result, "registered")

        mock_send_email.assert_called_once_with(
            "charles@example.com"
        )


if __name__ == "__main__":
    unittest.main()
```

Run:

```bash
python -m unittest -v test_registration.py
```

## What happens here?

This line is the key:

```python
@patch("registration.send_welcome_email")
```

It temporarily replaces the `send_welcome_email` name used by `registration.py` with a mock.

Then:

```python
def test_register_user(self, mock_send_email):
```

receives the mock as an argument.

The test calls the real `register_user()` function, but the email dependency is replaced.

Finally:

```python
mock_send_email.assert_called_once_with(
    "charles@example.com"
)
```

checks that the dependency was called correctly.

When the patched scope ends, the original object is restored.

## Using `patch()` as a context manager

You can also write:

```python
import unittest
from unittest.mock import patch

from registration import register_user


class TestRegistration(unittest.TestCase):

    def test_register_user(self):
        with patch("registration.send_welcome_email") as mock_send_email:
            result = register_user("charles@example.com")

        self.assertEqual(result, "registered")

        mock_send_email.assert_called_once_with(
            "charles@example.com"
        )
```

The `with` block limits the duration of the patch.

Use whichever form makes the test easier to read.

---

# 18. Understanding Where to Patch

This is one of the most important rules in mocking:

**Patch the name where the code under test looks it up, not necessarily where the object was originally defined.**

Suppose your project contains:

```text
project/
├── email_service.py
├── registration.py
└── test_registration.py
```

`email_service.py`:

```python
def send_welcome_email(email):
    print(f"Sending email to {email}")
```

`registration.py`:

```python
from email_service import send_welcome_email


def register_user(email):
    send_welcome_email(email)
```

Because `registration.py` imported the function into its own namespace, it uses the name:

```python
registration.send_welcome_email
```

Therefore, the test should patch:

```python
@patch("registration.send_welcome_email")
```

Not:

```python
@patch("email_service.send_welcome_email")
```

The second patch changes the name in `email_service`, but `registration.py` already has its own imported reference.

The general rule is:

- If the code uses `from module import function`, patch `module_where_the_function_is_used.function`.
- If the code uses `import module` and calls `module.function()`, patch the module name used by the code under test.

For example:

```python
import email_service


def register_user(email):
    email_service.send_welcome_email(email)
```

The appropriate target is:

```python
@patch("registration.email_service.send_welcome_email")
```

Here, `registration` is the module containing the code being tested.

---

# 19. Mocking Database Operations

Suppose you have a database service:

```python
# student_service.py

def get_student_name(repository, student_id: int) -> str:
    student = repository.get_by_id(student_id)

    if student is None:
        raise LookupError("Student not found.")

    return student["name"]
```

You can test this function without connecting to SQLite.

```python
import unittest
from unittest.mock import Mock

from student_service import get_student_name


class TestStudentService(unittest.TestCase):

    def setUp(self):
        self.repository = Mock()

    def test_student_found(self):
        self.repository.get_by_id.return_value = {
            "id": 1,
            "name": "Charles",
        }

        result = get_student_name(self.repository, 1)

        self.assertEqual(result, "Charles")

        self.repository.get_by_id.assert_called_once_with(1)

    def test_student_not_found(self):
        self.repository.get_by_id.return_value = None

        with self.assertRaises(LookupError):
            get_student_name(self.repository, 99)

        self.repository.get_by_id.assert_called_once_with(99)
```

## What are we testing?

The service expects an object with a `get_by_id()` method.

We replace that dependency with a mock repository.

For the first test:

```python
self.repository.get_by_id.return_value = {
    "id": 1,
    "name": "Charles",
}
```

The mock behaves as though a student was found.

For the second test:

```python
self.repository.get_by_id.return_value = None
```

The mock behaves as though no student exists.

This lets you test the service's behavior without requiring a real database.

However, it does not test the actual SQL query or database schema. Those require separate integration tests.

---

# 20. Mocking Email Functions

Imagine your application registers a user and sends a welcome email.

Your goal is to verify that the email function receives the correct address.

You do not need a real email server.

Example:

```python
# users.py

def send_email(email):
    # Real email delivery would happen here.
    ...


def register_user(email):
    # Save user information here.
    send_email(email)

    return {"email": email, "active": True}
```

Test:

```python
import unittest
from unittest.mock import patch

from