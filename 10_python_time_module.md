# Python `time` Module — Clocks, Delays, and Timestamps

## 1. What is `time`?
The `time` module provides functions for working with timestamps, delays, and clocks. For calendar dates and time zones, `datetime` is often more convenient.

```python
import time

print(time.time())  # seconds since Unix epoch, usually as a float
```

The Unix epoch is 1970-01-01 00:00:00 UTC.

## 2. `time.sleep(seconds)`

```python
import time

print("Starting")
time.sleep(2)
print("About two seconds later")
```

`sleep()` pauses the current thread for at least approximately the requested time. The actual delay can be longer due to scheduling and system load.

## 3. Wall-clock vs monotonic time

### `time.time()`
Returns a timestamp representing wall-clock time. It can change if the system clock is adjusted. Use it for timestamps and comparisons against real-world time when appropriate.

### `time.monotonic()`
A clock that cannot go backwards due to system-clock adjustments. Its reference point is undefined, so use differences, not calendar dates.

```python
import time

start = time.monotonic()
do_work = sum(range(1_000_000))
elapsed = time.monotonic() - start

print(f"Elapsed: {elapsed:.4f} seconds")
```

### `time.perf_counter()`
High-resolution performance counter, ideal for measuring short durations:

```python
start = time.perf_counter()
# code to measure
elapsed = time.perf_counter() - start
print(elapsed)
```

### `time.process_time()`
Measures CPU time consumed by the current process, excluding time spent sleeping. It is useful for CPU-work measurements rather than elapsed wall time.

## 4. Convert timestamps

```python
import time

timestamp = time.time()
local = time.localtime(timestamp)
utc = time.gmtime(timestamp)

print(time.ctime(timestamp))
print(time.strftime("%Y-%m-%d %H:%M:%S", local))
print(time.strftime("%Y-%m-%d %H:%M:%S", utc))
```

- `localtime()` converts to local time.
- `gmtime()` converts to UTC as a `struct_time`.
- `ctime()` returns a human-readable local-time representation.
- `strftime()` formats a time structure as text.

For timezone-aware application logic, use `datetime` with `zoneinfo` rather than relying on machine-local settings.

## 5. Parse a time string with `strptime()`

```python
import time

parsed = time.strptime("2026-10-31 14:30:00", "%Y-%m-%d %H:%M:%S")
print(parsed)
```

`strptime()` parses text according to a format. It returns a `struct_time`, not a timezone-aware datetime.

## 6. Common format codes

| Code | Meaning |
|---|---|
| `%Y` | Four-digit year |
| `%y` | Two-digit year |
| `%m` | Month, 01–12 |
| `%d` | Day of month |
| `%H` | Hour, 00–23 |
| `%I` | Hour, 01–12 |
| `%M` | Minute |
| `%S` | Second |
| `%p` | AM/PM marker (locale-dependent) |
| `%A` | Full weekday name |
| `%a` | Abbreviated weekday |
| `%B` | Full month name |
| `%b` | Abbreviated month |
| `%j` | Day of year |
| `%w` | Weekday number (Sunday is 0) |
| `%z` | UTC offset where supported by the time structure |
| `%Z` | Time zone name where available |
| `%%` | Literal percent sign |

Platform support for some format codes can vary.

## 7. Example: retry with a delay

```python
import time

for attempt in range(3):
    print(f"Attempt {attempt + 1}")
    # Try the operation here.
    time.sleep(1)
```

For network retries, use a bounded retry strategy and consider exponential backoff with jitter rather than retrying forever.

## 8. Common mistakes

- Using `time.time()` to measure durations when system-clock changes matter.
- Expecting `sleep(1)` to wake at exactly one second.
- Confusing a timestamp with a formatted date string.
- Assuming `struct_time` includes a full time-zone database.
- Using wall-clock time for performance benchmarks.

## 9. Practice
1. Measure a loop with `perf_counter()`.
2. Compare `time.time()` and `time.monotonic()`.
3. Format local and UTC time.
4. Parse a timestamp-like string using `strptime()`.
5. Build a retry loop with a maximum number of attempts.

**Key idea:** use `monotonic()` or `perf_counter()` to measure durations; use timestamps and `datetime` for real-world date/time work.
