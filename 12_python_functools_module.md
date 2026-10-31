# Python `functools` Module — Function Tools and Decorators

## 1. What is `functools`?
`functools` contains helpers for working with functions and callable objects. It is especially useful for decorators, caching, partial function application, and adapting call signatures.

```python
import functools
```

## 2. `functools.wraps()` — preserve function metadata

A decorator wraps one function with another. Without `wraps`, the wrapper can hide the original function's name and documentation.

```python
from functools import wraps

def announce(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print("Calling function")
        return func(*args, **kwargs)
    return wrapper

@announce
def greet(name: str) -> str:
    """Return a greeting."""
    return f"Hello, {name}"

print(greet("Charles"))
print(greet.__name__)  # greet
print(greet.__doc__)   # Return a greeting.
```

`wraps()` copies useful metadata and sets `__wrapped__` so introspection tools can reach the original function.

## 3. `functools.lru_cache()`

Caches results based on function arguments. Best for deterministic functions whose results can be reused.

```python
from functools import lru_cache

@lru_cache(maxsize=128)
def fibonacci(n: int) -> int:
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(30))
print(fibonacci.cache_info())
fibonacci.cache_clear()
```

Important:
- Arguments generally must be hashable.
- Do not cache functions that depend on changing external state unless you deliberately handle invalidation.
- Caches retain references to arguments and results until entries are cleared or evicted.
- `cache_info()` reports hits, misses, maximum size, and current size.

## 4. `functools.cache()`

`cache` is an unbounded cache, equivalent in behavior to `lru_cache(maxsize=None)`:

```python
from functools import cache

@cache
def square(n: int) -> int:
    return n * n
```

Because it is unbounded, consider memory growth if the function can receive many different arguments.

## 5. `functools.partial()`

Creates a callable with some arguments already filled in:

```python
from functools import partial

def power(base: float, exponent: float) -> float:
    return base ** exponent

square = partial(power, exponent=2)
cube = partial(power, exponent=3)

print(square(4))  # 16
print(cube(4))    # 64
```

Use `partial()` when you want a reusable specialized version of an existing function.

## 6. `functools.reduce()`

Combines an iterable into one result by repeatedly applying a two-argument function.

```python
from functools import reduce
from operator import mul

product = reduce(mul, [2, 3, 4], 1)
print(product)  # 24
```

For common operations such as sums, prefer clearer built-ins like `sum()` when possible.

## 7. `functools.total_ordering()`

Can fill in missing ordering methods for a class if it defines `__eq__()` and one ordering method such as `__lt__()`.

```python
from functools import total_ordering

@total_ordering
class Version:
    def __init__(self, number: int):
        self.number = number

    def __eq__(self, other):
        if not isinstance(other, Version):
            return NotImplemented
        return self.number == other.number

    def __lt__(self, other):
        if not isinstance(other, Version):
            return NotImplemented
        return self.number < other.number

print(Version(1) < Version(2))
print(Version(2) >= Version(1))
```

Generated methods may be slower than implementing all needed comparisons yourself. Define consistent comparison behavior.

## 8. `functools.singledispatch()`

Creates a generic function whose implementation is selected based on the type of its first argument.

```python
from functools import singledispatch

@singledispatch
def describe(value):
    return f"Other: {value!r}"

@describe.register
def _(value: int):
    return f"Integer: {value}"

@describe.register
def _(value: list):
    return f"List with {len(value)} items"

print(describe(5))
print(describe([1, 2, 3]))
print(describe("hello"))
```

Use it when a function needs clean type-specific behavior, not merely to replace a simple `if` statement.

## 9. `functools.partialmethod()`

Similar to `partial()`, but intended for methods in a class:

```python
from functools import partialmethod

class Greeter:
    def greet(self, greeting, name):
        return f"{greeting}, {name}!"

    hello = partialmethod(greet, "Hello")
    welcome = partialmethod(greet, "Welcome")

g = Greeter()
print(g.hello("Charles"))
print(g.welcome("Charles"))
```

## 10. Common mistakes and best practices

- Forgetting `@wraps(func)` in decorators.
- Caching functions that depend on current time, random values, or mutable external data.
- Using unbounded `cache` without considering memory.
- Passing unhashable arguments to cached functions.
- Using `reduce()` where `sum()` or a loop is clearer.
- Using `total_ordering` without consistent equality and ordering methods.

## 11. Quick reference

| Tool | Purpose |
|---|---|
| `wraps()` | Preserve wrapped function metadata |
| `lru_cache()` | Cache recent results with optional size limit |
| `cache()` | Unbounded memoization |
| `partial()` | Pre-fill function arguments |
| `partialmethod()` | Pre-fill method arguments |
| `reduce()` | Combine iterable into one value |
| `total_ordering()` | Generate missing comparison methods |
| `singledispatch()` | Dispatch based on first argument's type |

## 12. Practice
1. Write a decorator using `wraps()`.
2. Cache a slow deterministic function and inspect `cache_info()`.
3. Use `partial()` to create a specialized function.
4. Compare `reduce(mul, numbers, 1)` with a loop.
5. Create a type-specific function with `singledispatch()`.

**Key idea:** `functools` helps compose, adapt, decorate, and cache functions while keeping code reusable.
