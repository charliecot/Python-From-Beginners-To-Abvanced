# Python Containers and Collections — `list`, `tuple`, `set`, `dict`, and `collections`

## 1. What is a container?
A container is an object that holds or organizes other objects. Common built-in containers include:
- `list`: ordered, mutable sequence.
- `tuple`: ordered, immutable sequence.
- `set`: mutable collection of unique hashable elements, with no guaranteed positional order.
- `dict`: mapping of unique hashable keys to values.
- `str` and `bytes`: immutable sequences of characters or bytes.

## 2. Lists

```python
numbers = [10, 20, 20]
numbers.append(30)
numbers.extend([40, 50])
numbers.insert(0, 5)
numbers.remove(20)  # removes first matching value
last = numbers.pop()
print(numbers)
```

Lists are useful for ordered collections that may change. Common methods: `append`, `extend`, `insert`, `remove`, `pop`, `clear`, `index`, `count`, `sort`, `reverse`, `copy`.

## 3. Tuples

```python
point = (10, 20)
single = (5,)  # comma is required for a one-item tuple
x, y = point
```

Tuples are useful for fixed records and unpacking. A tuple is only hashable if all its elements are hashable.

## 4. Sets

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b)  # union
print(a & b)  # intersection
print(a - b)  # difference
print(a ^ b)  # symmetric difference
```

Sets remove duplicates and support fast membership checks. Empty set syntax is `set()`, not `{}` (`{}` creates an empty dictionary).

## 5. Dictionaries
A dictionary maps keys to values:

```python
user = {"name": "Charles", "active": True}
print(user["name"])
print(user.get("email", "not supplied"))
```

See the separate dictionary guide for detailed operations and patterns.

## 6. Choosing the right container

| Need | Choose | Why |
|---|---|---|
| Ordered collection that changes | `list` | Indexing and mutation |
| Fixed record / unpacking | `tuple` | Immutable sequence |
| Unique items / membership | `set` | Deduplication and set operations |
| Lookup by key | `dict` | Key-value mapping |
| FIFO work between threads | `queue.Queue` | Thread-safe producer/consumer queue |
| Count items | `collections.Counter` | Counting frequencies |
| Read-only view of a mapping | `types.MappingProxyType` | Prevents mutation through that view |

## 7. Useful `collections` tools

### `Counter`
```python
from collections import Counter
counts = Counter("banana")
print(counts)  # counts each character
```

### `defaultdict`
```python
from collections import defaultdict

groups = defaultdict(list)
groups["python"].append("re")
groups["python"].append("sys")
print(groups["python"])
```
A missing key automatically gets a value from the factory, here an empty list.

### `deque`
```python
from collections import deque

items = deque(["a", "b"])
items.append("c")
items.appendleft("start")
print(items.popleft())
```
A `deque` supports efficient additions/removals at both ends and can be used as a queue. For multi-threaded task tracking, use `queue.Queue` instead.

### `namedtuple`
```python
from collections import namedtuple

Point = namedtuple("Point", ["x", "y"])
p = Point(3, 4)
print(p.x, p.y)
```
For new code, `dataclasses.dataclass` is often a better fit for richer records, while `namedtuple` remains useful for lightweight immutable records.

## 8. Copying and mutability

```python
original = {"settings": {"theme": "dark"}}
shallow = original.copy()
shallow["settings"]["theme"] = "light"
print(original["settings"]["theme"])  # light
```

A shallow copy copies only the outer container. Nested objects are shared. Use `copy.deepcopy()` when a recursive copy is truly needed, while considering custom objects and cost.

## 9. Iterables, iterators, and generators

- **Iterable:** an object you can loop over, such as a list or string.
- **Iterator:** an object that supplies items one at a time with `next()`.
- **Generator:** a convenient way to create an iterator, often using `yield`.

```python
numbers = [1, 2, 3]
iterator = iter(numbers)
print(next(iterator))  # 1

def squares(limit):
    for n in range(limit):
        yield n * n

for value in squares(3):
    print(value)
```

Generators can save memory because they produce values as needed.

## 10. Common mistakes
- Using `{}` when you wanted an empty set.
- Expecting a set to preserve positional order.
- Forgetting the comma in a one-element tuple.
- Using a list as a dictionary key.
- Assuming a shallow copy duplicates nested objects.
- Choosing a list for repeated membership tests when a set may be more suitable.
- Using `deque` as if it provided all `queue.Queue` task-tracking behavior.

## 11. Practice
1. Remove duplicates from a list with `set()` while noting that set order is not a reliable substitute for original order.
2. Use a tuple to represent a coordinate.
3. Find union and intersection of two sets.
4. Count words with `Counter`.
5. Group values with `defaultdict(list)`.
6. Write a generator that yields filenames one at a time.

**Key idea:** choose a container based on ordering, mutability, uniqueness, lookup needs, and concurrency requirements.
