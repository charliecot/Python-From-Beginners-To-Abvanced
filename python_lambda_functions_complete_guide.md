# Python Lambda Functions: Complete Guide (Beginner to Advanced)

A detailed guide to lambda expressions, normal functions, arguments, return values, scope, higher-order functions, sorting, filtering, mapping, callbacks, closures, and practical examples.

---

## Table of Contents

1. What is a function?
2. What is a lambda function?
3. Lambda syntax
4. Lambda compared with `def`
5. Arguments and parameters
6. Return values
7. Expressions and limitations
8. Assigning lambdas to variables
9. Lambda with `map()`
10. Lambda with `filter()`
11. Lambda with `sorted()`
12. Lambda with `min()` and `max()`
13. Lambda with `reduce()`
14. Lambda with `any()` and `all()`
15. Lambda with lists, tuples, and dictionaries
16. Lambda with `key=`
17. Lambda with conditional expressions
18. Lambda with functions as arguments
19. Callbacks and event-driven code
20. Closures and variable scope
21. Late binding and default arguments
22. Lambda in decorators
23. Type hints and lambda functions
24. Practical networking and cybersecurity examples
25. Common mistakes
26. Best practices: when to use `lambda` and when to use `def`
27. Quick reference
28. Practice exercises

---

# 1. What Is a Function?

Before understanding `lambda`, understand an ordinary Python function.

A function is a reusable block of code that performs a task.

```python
def greet():
    print("Hello!")

greet()
```

Output:

```text
Hello!
```

Here is a function that receives information:

```python
def greet(name):
    print(f"Hello, {name}!")

greet("Charles")
```

Output:

```text
Hello, Charles!
```

A function can also calculate something and return a result:

```python
def add(a, b):
    result = a + b
    return result

answer = add(10, 5)

print(answer)
```

Output:

```text
15
```

Important terms:

| Term | Meaning |
|---|---|
| `def` | Keyword used to define a normal function |
| Function name | The name used to refer to the function |
| Parameter | A variable in the function definition |
| Argument | A value passed when calling the function |
| `return` | Sends a result back to the caller |
| Function call | Executes the function |

---

# 2. What Is a Lambda Function?

A lambda function is a small anonymous function created using the `lambda` keyword.

Anonymous means that the function does not need a declared name.

Example:

```python
lambda x: x * 2
```

Read this as:

"Create a function that receives `x` and returns `x * 2`."

The lambda expression creates a function object. It does not execute the function yet.

To execute it immediately:

```python
result = (lambda x: x * 2)(5)

print(result)
```

Output:

```text
10
```

Notice the two operations:

1. `lambda x: x * 2` creates a function.
2. `(5)` calls that function with the argument `5`.

---

# 3. Lambda Syntax

General syntax:

```python
lambda parameters: expression
```

Breakdown:

```python
lambda x: x + 10
```

| Part | Meaning |
|---|---|
| `lambda` | Creates a lambda function |
| `x` | Parameter |
| `:` | Separates parameters from the expression |
| `x + 10` | Expression whose result is returned |

A lambda can receive multiple parameters:

```python
lambda a, b: a + b
```

It can also receive no parameters:

```python
lambda: "Hello"
```

And it can receive a single parameter:

```python
lambda name: f"Hello, {name}"
```

Examples:

```python
double = lambda x: x * 2
add = lambda a, b: a + b
greet = lambda: "Hello"

print(double(5))       # 10
print(add(10, 5))      # 15
print(greet())         # Hello
```

Although these assignments work, a normal `def` is usually clearer when you want to give a function a reusable name.

---

# 4. Lambda Compared with `def`

Both can create function objects, but their syntax and capabilities differ.

## 4.1 Normal function

```python
def multiply(a, b):
    result = a * b
    return result
```

## 4.2 Lambda equivalent

```python
multiply = lambda a, b: a * b
```

Both can be called:

```python
print(multiply(5, 4))
```

Output:

```text
20
```

Comparison:

| Feature | `def` | `lambda` |
|---|---|---|
| Creates a function | Yes | Yes |
| Can accept parameters | Yes | Yes |
| Returns a value | With `return` | Automatically returns expression result |
| Multiple statements | Yes | No |
| Docstring | Yes | No normal function docstring syntax |
| Type annotations | Full support | Limited inline annotation syntax |
| Best use | Reusable, complex logic | Short, simple expressions |

## 4.3 Why does lambda not need `return`?

With `def`:

```python
def square(number):
    return number ** 2
```

With `lambda`:

```python
square = lambda number: number ** 2
```

The expression after the colon is evaluated and its value is returned automatically.

This is called an **implicit return**.

You cannot write a normal `return` statement inside a lambda:

```python
# Invalid:
# square = lambda number: return number ** 2
```

Correct:

```python
square = lambda number: number ** 2
```

---

# 5. Lambda Arguments and Parameters

Lambda functions support the same common parameter styles as normal functions.

## 5.1 One parameter

```python
square = lambda number: number ** 2

print(square(5))
```

## 5.2 Multiple parameters

```python
add = lambda a, b: a + b

print(add(10, 20))
```

## 5.3 Default parameters

```python
greet = lambda name="Guest": f"Hello, {name}!"

print(greet())
print(greet("Charles"))
```

Output:

```text
Hello, Guest!
Hello, Charles!
```

## 5.4 Keyword arguments

```python
divide = lambda a, b: a / b

print(divide(a=20, b=4))
```

Output:

```text
5.0
```

## 5.5 Variable-length positional arguments: `*args`

```python
total = lambda *numbers: sum(numbers)

print(total(10, 20, 30, 40))
```

Output:

```text
100
```

`*numbers` collects positional arguments into a tuple.

## 5.6 Variable-length keyword arguments: `**kwargs`

```python
show_user = lambda **details: details

user = show_user(name="Charles", age=25, country="Cameroon")

print(user)
```

Output:

```text
{'name': 'Charles', 'age': 25, 'country': 'Cameroon'}
```

`**details` collects keyword arguments into a dictionary.

---

# 6. Return Values

A lambda returns the value of its single expression.

```python
get_status = lambda code: "Success" if code == 200 else "Error"

status = get_status(200)

print(status)
```

Output:

```text
Success
```

You can return any kind of object as the expression result:

```python
get_list = lambda: [10, 20, 30]
get_tuple = lambda: (10, 20, 30)
get_dict = lambda: {"name": "Charles", "active": True}

print(get_list())
print(get_tuple())
print(get_dict())
```

A lambda can even return another function:

```python
def make_multiplier(factor):
    return lambda number: number * factor

double = make_multiplier(2)
triple = make_multiplier(3)

print(double(10))  # 20
print(triple(10))  # 30
```

This introduces closures, which are discussed later.

---

# 7. Lambda Expressions and Limitations

A lambda body must be an expression, not a block of statements.

Valid expressions include:

```python
lambda x: x + 5
lambda x: x > 10
lambda x: x.upper()
lambda x: len(x)
lambda x: [item for item in x]
lambda x: {"value": x}
lambda x: "Positive" if x > 0 else "Non-positive"
```

Examples:

```python
get_length = lambda text: len(text)
is_even = lambda number: number % 2 == 0
make_upper = lambda text: text.upper()

print(get_length("Python"))  # 6
print(is_even(10))           # True
print(make_upper("hello"))   # HELLO
```

A normal lambda cannot contain statements such as:

- Assignment statements (`x = 10`)
- `return` statements
- `for` statements as a normal block
- `while` statements
- `try`/`except` statements
- `import` statements

Use `def` when the operation requires multiple statements, exception handling, loops with side effects, or meaningful documentation.

A comprehension is an expression, so it can be used:

```python
get_squares = lambda numbers: [number ** 2 for number in numbers]

print(get_squares([1, 2, 3, 4]))
```

Output:

```text
[1, 4, 9, 16]
```

---

# 8. Assigning Lambda Functions to Variables

You can assign a lambda to a variable:

```python
square = lambda x: x ** 2

print(square(5))
```

But understand the distinction:

```python
square = lambda x: x ** 2
```

This assigns a function object to `square`.

It does not calculate the square yet.

The calculation happens when you call:

```python
square(5)
```

For a reusable function, prefer:

```python
def square(x):
    return x ** 2
```

Use a lambda primarily when a short function is needed temporarily or passed as an argument.

---

# 9. Lambda with `map()`

`map()` applies a function to every item in an iterable.

Syntax:

```python
map(function, iterable)
```

It returns a map iterator. Convert it to a list to see all results immediately.

## 9.1 Example without lambda

```python
def double(number):
    return number * 2

numbers = [1, 2, 3, 4]

result = map(double, numbers)

print(list(result))
```

Output:

```text
[2, 4, 6, 8]
```

## 9.2 Example with lambda

```python
numbers = [1, 2, 3, 4]

result = map(lambda number: number * 2, numbers)

print(list(result))
```

Output:

```text
[2, 4, 6, 8]
```

The lambda defines the small operation directly where `map()` needs it.

## 9.3 Convert strings to uppercase

```python
names = ["charles", "john", "mary"]

result = map(lambda name: name.upper(), names)

print(list(result))
```

## 9.4 Convert values to hexadecimal

```python
numbers = [10, 15, 16, 255]

result = map(lambda number: f"{number:02X}", numbers)

print(list(result))
```

Output:

```text
['0A', '0F', '10', 'FF']
```

## 9.5 `map()` with multiple iterables

```python
numbers_a = [1, 2, 3]
numbers_b = [10, 20, 30]

result = map(lambda a, b: a + b, numbers_a, numbers_b)

print(list(result))
```

Output:

```text
[11, 22, 33]
```

The function receives one item from each iterable at a time. Iteration stops when the shortest iterable is exhausted.

## 9.6 When should you use `map()`?

Use it when you want to transform every item in an iterable.

Examples:

- Convert all names to uppercase.
- Convert integers to hexadecimal strings.
- Extract a property from every object.
- Apply a calculation to every value.

A list comprehension is often clearer for simple transformations:

```python
numbers = [1, 2, 3, 4]

result = [number * 2 for number in numbers]
```

---

# 10. Lambda with `filter()`

`filter()` selects items from an iterable based on a function.

Syntax:

```python
filter(function, iterable)
```

The function should return a truthy or falsy result.

- Truthy: keep the item.
- Falsy: discard the item.

## 10.1 Select even numbers

```python
numbers = [1, 2, 3, 4, 5, 6]

result = filter(lambda number: number % 2 == 0, numbers)

print(list(result))
```

Output:

```text
[2, 4, 6]
```

Understand the expression:

```python
number % 2 == 0
```

If the remainder after division by 2 is zero, the number is even.

## 10.2 Select odd numbers

```python
numbers = [1, 2, 3, 4, 5, 6]

result = filter(lambda number: number % 2 != 0, numbers)

print(list(result))
```

## 10.3 Filter names by length

```python
names = ["John", "Charles", "Mary", "Christopher"]

result = filter(lambda name: len(name) > 4, names)

print(list(result))
```

## 10.4 Filter open ports

```python
ports = [21, 22, 23, 53, 80, 443, 8080]

common_ports = {22, 53, 80, 443}

result = filter(lambda port: port in common_ports, ports)

print(list(result))
```

Output:

```text
[22, 53, 80, 443]
```

## 10.5 Filter dictionaries

```python
users = [
    {"name": "Charles", "active": True},
    {"name": "John", "active": False},
    {"name": "Mary", "active": True},
]

active_users = filter(lambda user: user["active"], users)

print(list(active_users))
```

## 10.6 Filter with a list comprehension

Equivalent:

```python
numbers = [1, 2, 3, 4, 5, 6]

result = [number for number in numbers if number % 2 == 0]

print(result)
```

---

# 11. Lambda with `sorted()`

`sorted()` sorts an iterable and returns a new list.

Syntax:

```python
sorted(iterable, key=None, reverse=False)
```

Important parameters:

| Parameter | Meaning |
|---|---|
| `iterable` | The values to sort |
| `key` | A function that calculates the sorting key |
| `reverse` | `True` for descending order |

## 11.1 Sort numbers

```python
numbers = [5, 2, 8, 1, 9]

result = sorted(numbers)

print(result)
```

## 11.2 Sort strings by length

```python
names = ["Christopher", "John", "Mary", "Alex"]

result = sorted(names, key=lambda name: len(name))

print(result)
```

Output:

```text
['John', 'Mary', 'Alex', 'Christopher']
```

The `key` function calculates the value used for comparison.

It does not replace the original item in the output.

## 11.3 Sort in descending order

```python
names = ["Christopher", "John", "Mary", "Alex"]

result = sorted(names, key=lambda name: len(name), reverse=True)

print(result)
```

## 11.4 Sort dictionaries by age

```python
users = [
    {"name": "Charles", "age": 25},
    {"name": "John", "age": 19},
    {"name": "Mary", "age": 30},
]

result = sorted(users, key=lambda user: user["age"])

for user in result:
    print(user)
```

## 11.5 Sort dictionaries by multiple fields

```python
users = [
    {"name": "Charles", "age": 25},
    {"name": "John", "age": 19},
    {"name": "Mary", "age": 25},
]

result = sorted(
    users,
    key=lambda user: (user["age"], user["name"])
)

print(result)
```

This sorts by age first. If two users have the same age, it sorts by name.

## 11.6 Sort by the last character

```python
names = ["Charles", "John", "Mary", "Alex"]

result = sorted(names, key=lambda name: name[-1])

print(result)
```

## 11.7 `sorted()` versus `.sort()`

```python
numbers = [5, 2, 8, 1]

new_list = sorted(numbers)

print(new_list)
print(numbers)
```

`sorted()` returns a new list.

```python
numbers = [5, 2, 8, 1]

numbers.sort(key=lambda number: number)

print(numbers)
```

`.sort()` changes the original list and returns `None`.

---

# 12. Lambda with `min()` and `max()`

The `key` parameter can tell `min()` and `max()` how to compare objects.

## 12.1 Find the longest name

```python
names = ["John", "Christopher", "Mary", "Alex"]

longest = max(names, key=lambda name: len(name))

print(longest)
```

## 12.2 Find the shortest name

```python
shortest = min(names, key=lambda name: len(name))

print(shortest)
```

## 12.3 Find the user with the highest age

```python
users = [
    {"name": "Charles", "age": 25},
    {"name": "John", "age": 19},
    {"name": "Mary", "age": 30},
]

oldest = max(users, key=lambda user: user["age"])

print(oldest)
```

## 12.4 Find the port closest to a target

```python
ports = [21, 22, 80, 443, 8080]
target = 100

closest = min(ports, key=lambda port: abs(port - target))

print(closest)
```

The `key` function calculates the absolute distance from the target.

---

# 13. Lambda with `reduce()`

`reduce()` repeatedly combines items to produce one final result.

It is available from `functools`.

```python
from functools import reduce
```

Syntax:

```python
reduce(function, iterable, initial_value)
```

## 13.1 Add all numbers

```python
from functools import reduce

numbers = [1, 2, 3, 4, 5]

result = reduce(lambda a, b: a + b, numbers)

print(result)
```

Output:

```text
15
```

Understand the process:

```text
First:  1 + 2 = 3
Next:   3 + 3 = 6
Next:   6 + 4 = 10
Last:  10 + 5 = 15
```

The result from one operation becomes the first argument of the next operation.

## 13.2 Multiply all numbers

```python
from functools import reduce

numbers = [2, 3, 4]

result = reduce(lambda a, b: a * b, numbers)

print(result)
```

Output:

```text
24
```

## 13.3 Use an initial value

```python
from functools import reduce

numbers = [1, 2, 3, 4]

result = reduce(lambda a, b: a + b, numbers, 10)

print(result)
```

Output:

```text
20
```

The initial value is 10:

```text
10 + 1 = 11
11 + 2 = 13
13 + 3 = 16
16 + 4 = 20
```

## 13.4 Why not always use `reduce()`?

For addition:

```python
sum(numbers)
```

is clearer than:

```python
reduce(lambda a, b: a + b, numbers)
```

Use `reduce()` when you genuinely need to combine a sequence using a custom two-value operation.

---

# 14. Lambda with `any()` and `all()`

These built-in functions test conditions across iterables.

## 14.1 `any()`

Returns `True` if at least one item is truthy.

```python
numbers = [1, 3, 5, 8]

result = any(map(lambda number: number % 2 == 0, numbers))

print(result)
```

Output:

```text
True
```

At least one number is even.

## 14.2 `all()`

Returns `True` only if every item is truthy.

```python
numbers = [2, 4, 6, 8]

result = all(map(lambda number: number % 2 == 0, numbers))

print(result)
```

Output:

```text
True
```

## 14.3 More readable generator expressions

You can often skip `map()`:

```python
numbers = [2, 4, 6, 8]

result = all(number % 2 == 0 for number in numbers)

print(result)
```

---

# 15. Lambda with Lists, Tuples, and Dictionaries

## 15.1 List of tuples

```python
students = [
    ("Charles", 80),
    ("John", 65),
    ("Mary", 90),
]

result = sorted(students, key=lambda student: student[1])

print(result)
```

`student[1]` selects the second element, the score.

## 15.2 List of dictionaries

```python
devices = [
    {"name": "PC-01", "ip": "192.168.1.10", "priority": 2},
    {"name": "PC-02", "ip": "192.168.1.20", "priority": 1},
    {"name": "PC-03", "ip": "192.168.1.30", "priority": 3},
]

result = sorted(devices, key=lambda device: device["priority"])

print(result)
```

## 15.3 Transform dictionary values

```python
scores = {
    "Charles": 80,
    "John": 65,
    "Mary": 90,
}

result = dict(map(
    lambda item: (item[0], item[1] + 5),
    scores.items()
))

print(result)
```

Output:

```text
{'Charles': 85, 'John': 70, 'Mary': 95}
```

A dictionary comprehension may be clearer:

```python
result = {name: score + 5 for name, score in scores.items()}
```

## 15.4 Sort a dictionary's items

```python
scores = {
    "Charles": 80,
    "John": 65,
    "Mary": 90,
}

result = sorted(scores.items(), key=lambda item: item[1])

print(result)
```

`items()` returns key-value pairs such as:

```python
("Charles", 80)
```

---

# 16. Understanding the `key=` Parameter

This is one of the most important concepts when using lambda with Python's sorting and comparison functions.

Consider:

```python
names = ["John", "Christopher", "Mary"]

result = sorted(names, key=lambda name: len(name))
```

What happens?

1. `sorted()` takes an item.
2. It calls the key function with that item.
3. The lambda returns the item's length.
4. `sorted()` compares those returned lengths.
5. It returns the original names in the resulting order.

Conceptual demonstration:

| Original item | Key result |
|---|---:|
| John | 4 |
| Christopher | 11 |
| Mary | 4 |

The key function does not return the sorted item. It returns the value used to compare items.

You can use `key=` with:

- `sorted()`
- `list.sort()`
- `min()`
- `max()`
- `heapq` functions that accept a key (depending on the function)

---

# 17. Lambda with Conditional Expressions

A conditional expression allows a simple `if/else` inside a lambda.

Syntax:

```python
value_if_true if condition else value_if_false
```

Example:

```python
check_number = lambda number: "Even" if number % 2 == 0 else "Odd"

print(check_number(10))
print(check_number(7))
```

Output:

```text
Even
Odd
```

Another example:

```python
check_age = lambda age: "Adult" if age >= 18 else "Minor"

print(check_age(25))
print(check_age(15))
```

Multiple branches can be chained, but they can become difficult to read:

```python
grade = lambda score: (
    "A" if score >= 80 else
    "B" if score >= 70 else
    "C" if score >= 60 else
    "F"
)
```

If conditional logic becomes complicated, use `def` with normal `if`/`elif`/`else` statements.

---

# 18. Functions as Arguments: Higher-Order Functions

A higher-order function is a function that:

1. Accepts another function as an argument, or
2. Returns another function.

Python functions are objects. You can pass them around just like other values.

## 18.1 Pass a normal function

```python
def apply_operation(function, value):
    return function(value)

def square(number):
    return number ** 2

result = apply_operation(square, 5)

print(result)
```

Output:

```text
25
```

Notice:

```python
apply_operation(square, 5)
```

You pass `square`, not `square()`.

- `square` means the function object.
- `square()` calls the function.

## 18.2 Pass a lambda

```python
def apply_operation(function, value):
    return function(value)

result = apply_operation(lambda number: number ** 2, 5)

print(result)
```

Output:

```text
25
```

The lambda is created and passed to `apply_operation()`. That function then calls it.

## 18.3 Build a reusable operation function

```python
def calculate(a, b, operation):
    return operation(a, b)

addition = calculate(10, 5, lambda x, y: x + y)
multiplication = calculate(10, 5, lambda x, y: x * y)

print(addition)
print(multiplication)
```

This is useful when the operation should be supplied by the caller.

---

# 19. Lambda with Callbacks and Event-Driven Code

A callback is a function passed to another function so that it can be called later.

Example:

```python
def process_data(data, callback):
    result = data.upper()
    callback(result)

process_data(
    "hello",
    lambda result: print(f"Processed: {result}")
)
```

Output:

```text
Processed: HELLO
```

The callback is:

```python
lambda result: print(f"Processed: {result}")
```

It is passed to `process_data()` and called when processing is complete.

Common uses of callbacks:

- GUI button actions
- Event handlers
- Sorting functions
- Asynchronous operations
- Network event handling
- Scheduling tasks

A lambda is useful when the callback is short.

Use a named function when the callback is complex or needs to be reused.

---

# 20. Closures and Variable Scope

A closure is a function that remembers variables from the enclosing scope in which it was created.

Lambdas can form closures too.

Example:

```python
def make_multiplier(factor):
    return lambda number: number * factor

double = make_multiplier(2)

print(double(10))
```

Output:

```text
20
```

Understand the steps:

1. `make_multiplier(2)` is called.
2. `factor` receives `2`.
3. The lambda is created.
4. The lambda refers to `factor`.
5. The lambda is returned.
6. `double(10)` calls the returned lambda.
7. It calculates `10 * 2`.

The returned lambda retains access to the enclosing variable `factor`.

## 20.1 Create different closures

```python
def make_multiplier(factor):
    return lambda number: number * factor

double = make_multiplier(2)
triple = make_multiplier(3)
quadruple = make_multiplier(4)

print(double(5))      # 10
print(triple(5))      # 15
print(quadruple(5))   # 20
```

Each returned function remembers its own `factor` binding.

## 20.2 Closure with a prefix

```python
def make_greeter(prefix):
    return lambda name: f"{prefix}, {name}!"

hello = make_greeter("Hello")
welcome = make_greeter("Welcome")

print(hello("Charles"))
print(welcome("Charles"))
```

---

# 21. Late Binding and Default Arguments

This is an important advanced topic.

Consider:

```python
functions = []

for number in range(3):
    functions.append(lambda: number)

for function in functions:
    print(function())
```

You might expect:

```text
0
1
2
```

But the result is:

```text
2
2
2
```

Why?

The lambdas refer to the variable `number`. They do not capture a separate copy of its value at each loop iteration.

When the functions are called, the loop has already finished, and `number` is `2`.

This behavior is called **late binding**.

## 21.1 Fix using a default argument

```python
functions = []

for number in range(3):
    functions.append(lambda number=number: number)

for function in functions:
    print(function())
```

Output:

```text
0
1
2
```

Why does this work?

Default argument values are evaluated when the function is created. Each lambda therefore receives the current value as its default.

## 21.2 Another example

Problem:

```python
operations = []

for factor in [2, 3, 4]:
    operations.append(lambda number: number * factor)

for operation in operations:
    print(operation(10))
```

All three operations use the final `factor` value.

Fix:

```python
operations = []

for factor in [2, 3, 4]:
    operations.append(
        lambda number, factor=factor: number * factor
    )

for operation in operations:
    print(operation(10))
```

Output:

```text
20
30
40
```

This is especially important when creating callbacks inside loops.

---

# 22. Lambda in Decorators

A decorator is a function that receives another function and returns a replacement or wrapped function.

A decorator is normally implemented with `def`.

A simple lambda can be returned from a decorator-like function, but do not force complex decorator logic into lambdas.

Example:

```python
def add_prefix(function):
    return lambda name: f"Hello, {function(name)}"

def get_name(name):
    return name.upper()

greet = add_prefix(get_name)

print(greet("Charles"))
```

Output:

```text
Hello, CHARLES
```

A more conventional decorator:

```python
from functools import wraps

def add_prefix(function):
    @wraps(function)
    def wrapper(name):
        return f"Hello, {function(name)}"

    return wrapper

@add_prefix
def get_name(name):
    return name.upper()

print(get_name("Charles"))
```

Use `def` for decorators that need multiple statements, metadata preservation, exception handling, or flexible arguments.

---

# 23. Type Hints and Lambda Functions

Normal functions support parameter and return annotations:

```python
def square(number: int) -> int:
    return number ** 2
```

A lambda expression can be assigned to a variable with a type annotation:

```python
from collections.abc import Callable

square: Callable[[int], int] = lambda number: number ** 2
```

Understand the type:

```python
Callable[[int], int]
```

It means:

- The function accepts an integer argument.
- The function returns an integer.

A function that receives two integers and returns an integer:

```python
add: Callable[[int, int], int] = lambda a, b: a + b
```

Type annotations help tools and readers understand expected types. They do not automatically enforce types at runtime.

For public, reusable, or complex functions, a normal `def` with annotations is usually easier to understand.

---

# 24. Practical Networking and Cybersecurity Examples

## 24.1 Convert bytes to hexadecimal

```python
packet = b"\x0A\x1B\xFF"

hex_values = list(map(lambda byte: f"{byte:02X}", packet))

print(hex_values)
```

Output:

```text
['0A', '1B', 'FF']
```

## 24.2 Format a MAC address from bytes

```python
mac_bytes = bytes([164, 91, 124, 45, 158, 241])

mac_address = ":".join(map(lambda byte: f"{byte:02X}", mac_bytes))

print(mac_address)
```

Output:

```text
A4:5B:7C:2D:9E:F1
```

## 24.3 Filter suspicious ports from a list

This is only a simple demonstration of filtering; a port number alone does not prove malicious activity.

```python
ports = [22, 80, 443, 4444, 8080, 31337]

unusual_ports = list(
    filter(lambda port: port not in {22, 80, 443, 8080}, ports)
)

print(unusual_ports)
```

## 24.4 Sort network devices by IP address

Sorting dotted IPv4 strings alphabetically can produce incorrect numerical ordering. Convert each address to an integer first.

```python
import ipaddress

devices = [
    {"name": "PC-01", "ip": "192.168.1.20"},
    {"name": "PC-02", "ip": "192.168.1.3"},
    {"name": "PC-03", "ip": "192.168.1.100"},
]

sorted_devices = sorted(
    devices,
    key=lambda device: int(ipaddress.ip_address(device["ip"]))
)

for device in sorted_devices:
    print(device)
```

## 24.5 Extract active devices

```python
devices = [
    {"ip": "192.168.1.10", "active": True},
    {"ip": "192.168.1.20", "active": False},
    {"ip": "192.168.1.30", "active": True},
]

active_devices = list(
    filter(lambda device: device["active"], devices)
)

print(active_devices)
```

## 24.6 Create a small packet transformation pipeline

```python
packet = bytes([10, 20, 30, 40, 255])

# Keep bytes greater than or equal to 20.
filtered = filter(lambda byte: byte >= 20, packet)

# Convert the remaining bytes to uppercase hexadecimal.
formatted = map(lambda byte: f"{byte:02X}", filtered)

print(list(formatted))
```

Output:

```text
['14', '1E', '28', 'FF']
```

---

# 25. Common Mistakes

## Mistake 1: Forgetting that lambda creates a function

```python
double = lambda number: number * 2
```

This creates a function. It does not calculate a result until called.

```python
print(double(5))
```

## Mistake 2: Trying to use `return`

Incorrect:

```python
# lambda x: return x * 2
```

Correct:

```python
lambda x: x * 2
```

## Mistake 3: Using `print()` when you need a returned value

```python
show = lambda number: print(number * 2)

result = show(5)

print(result)
```

Output:

```text
10
None
```

Why?

`print()` displays a value but returns `None`.

If you need the calculated value:

```python
double = lambda number: number * 2

result = double(5)

print(result)
```

## Mistake 4: Forgetting to consume iterators

`map()` and `filter()` return iterators.

```python
numbers = [1, 2, 3]

result = map(lambda number: number * 2, numbers)

print(result)
```

This displays a map object, not the resulting list.

Use:

```python
print(list(result))
```

Remember that iterators are consumed as you iterate over them. Once exhausted, they do not automatically restart.

## Mistake 5: Making lambda too complicated

Avoid:

```python
process = lambda x: [item.upper() if item else "UNKNOWN" for item in x if isinstance(item, str)]
```

If logic is complex, use a named function.

## Mistake 6: Confusing a function with a function call

```python
def square(number):
    return number ** 2

# Pass the function:
operation = square

# Call the function:
result = square(5)
```

These are different operations.

---

# 26. Best Practices: When to Use `lambda` and When to Use `def`

## Use lambda when:

- You need a short function for `sorted(key=...)`.
- You need a simple transformation with `map()`.
- You need a simple condition with `filter()`.
- You need a small callback.
- You need a short function to return from another function.
- The expression is easy to understand immediately.

Example:

```python
users = [
    {"name": "Charles", "age": 25},
    {"name": "John", "age": 19},
]

result = sorted(users, key=lambda user: user["age"])
```

## Use `def` when:

- The operation needs multiple statements.
- You need loops or exception handling.
- The logic is complex.
- You need a docstring.
- The function will be reused.
- You need detailed type annotations.
- You want a clear stack trace and meaningful function name.

Example:

```python
def validate_port(port: int) -> bool:
    """Return whether a port number is in the valid TCP/UDP range."""
    if not isinstance(port, int):
        return False

    return 0 <= port <= 65535
```

## Comparison of common tools

| Tool | Purpose |
|---|---|
| `map()` | Transform every item |
| `filter()` | Keep items that pass a condition |
| `sorted()` | Return a sorted list |
| `.sort()` | Sort a list in place |
| `min()` | Find the smallest item |
| `max()` | Find the largest item |
| `reduce()` | Combine items into one result |
| `any()` | Check whether at least one item is truthy |
| `all()` | Check whether every item is truthy |
| `Callable` | Describe a callable type in type hints |

---

# 27. Quick Reference

```python
# Basic lambda
lambda x: x * 2

# Multiple parameters
lambda a, b: a + b

# Default parameter
lambda name="Guest": name

# Conditional expression
lambda x: "Even" if x % 2 == 0 else "Odd"

# map: transform values
map(lambda x: x * 2, numbers)

# filter: select values
filter(lambda x: x > 10, numbers)

# sorted: sort by a calculated key
sorted(names, key=lambda name: len(name))

# max: find the item with the greatest key
max(users, key=lambda user: user["age"])

# min: find the item with the smallest key
min(users, key=lambda user: user["age"])

# reduce: combine values
reduce(lambda a, b: a + b, numbers)

# any: at least one condition is true
any(map(lambda x: x > 10, numbers))

# all: every condition is true
all(map(lambda x: x > 10, numbers))

# Closure
def make_multiplier(factor):
    return lambda number: number * factor

# Avoid late binding in a loop
functions = [lambda x=i: x for i in range(3)]

# Type hint
operation: Callable[[int], int] = lambda x: x * 2
```

---

# 28. Practice Exercises

Try to solve these before checking the solutions.

## Beginner

### Exercise 1: Double a number

Create a lambda that receives a number and returns twice its value.

Expected:

```text
20
```

when called with `10`.

### Exercise 2: Add two numbers

Create a lambda that receives two numbers and returns their sum.

### Exercise 3: Check even numbers

Create a lambda that returns `True` when a number is even and `False` otherwise.

### Exercise 4: Convert to uppercase

Use a lambda to convert a string to uppercase.

## Intermediate

### Exercise 5: Use `map()`

Given:

```python
numbers = [1, 2, 3, 4, 5]
```

Use `map()` and lambda to calculate the square of every number.

Expected:

```text
[1, 4, 9, 16, 25]
```

### Exercise 6: Use `filter()`

Given:

```python
numbers = [10, 15, 20, 25, 30, 35]
```

Keep only numbers greater than 20.

### Exercise 7: Use `sorted()`

Given:

```python
names = ["Christopher", "John", "Mary", "Alex"]
```

Sort the names by their length.

### Exercise 8: Sort dictionaries

Given:

```python
users = [
    {"name": "Charles", "age": 25},
    {"name": "John", "age": 19},
    {"name": "Mary", "age": 30},
]
```

Sort the users by age.

### Exercise 9: Find the longest string

Use `max()` and lambda to find the longest name.

## Advanced

### Exercise 10: Use `reduce()`

Use `reduce()` to multiply all numbers:

```python
numbers = [2, 3, 4, 5]
```

Expected:

```text
120
```

### Exercise 11: Build a closure

Create a function called `make_power()` that accepts an exponent and returns a lambda.

Expected:

```python
square = make_power(2)
cube = make_power(3)

print(square(5))  # 25
print(cube(5))    # 125
```

### Exercise 12: Understand late binding

Predict the output:

```python
functions = []

for number in range(4):
    functions.append(lambda: number)

print([function() for function in functions])
```

Then correct the code so that it returns:

```text
[0, 1, 2, 3]
```

### Exercise 13: Networking

Given:

```python
packet = bytes([10, 20, 30, 255])
```

Use `map()` and lambda to convert each byte to two-character uppercase hexadecimal.

Expected:

```text
['0A', '14', '1E', 'FF']
```

### Exercise 14: Combine `filter()` and `map()`

Given:

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8]
```

1. Keep only even numbers.
2. Multiply each remaining number by 10.

Expected:

```text
[20, 40, 60, 80]
```

---

# Final Summary

The key idea is simple:

```python
lambda parameters: expression
```

A lambda creates a function that evaluates one expression and returns its result.

For example:

```python
double = lambda number: number * 2

print(double(5))
```

Output:

```text
10
```

The most important practical pattern is passing a lambda to another function:

```python
sorted(users, key=lambda user: user["age"])
```

Here, `sorted()` needs a function that tells it which value to use when comparing users.

Remember these distinctions:

- `lambda` creates a small function.
- `map()` transforms items.
- `filter()` selects items.
- `sorted()` orders items.
- `reduce()` combines items.
- `key=` supplies a function used for comparison.
- A closure remembers variables from its enclosing scope.
- Default arguments can avoid late-binding surprises in loops.
- `def` is preferable for complex or reusable logic.

**Official Python documentation:**

- Lambda expressions: https://docs.python.org/3/reference/expressions.html#lambda
- Built-in functions (`map`, `filter`, `sorted`, `min`, `max`, `any`, `all`): https://docs.python.org/3/library/functions.html
- `functools.reduce()`: https://docs.python.org/3/library/functools.html#functools.reduce
- Callable type hints: https://docs.python.org/3/library/typing.html
