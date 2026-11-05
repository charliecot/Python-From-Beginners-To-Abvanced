# PYTHON SUBPROCESS AND THREADING — COMPLETE DOCUMENTATION

**Author:** Nzegge Charles Ngong  
**Language:** Python  
**Level:** Beginner to Advanced

---

# PART 1: THE SUBPROCESS MODULE

## 1. What is subprocess?

The `subprocess` module allows Python to start and communicate with external programs and operating-system commands.

For example, Python can execute:

- Linux commands: `ls`, `pwd`, `ping`, `ip`, `whoami`.
- Windows commands: `ipconfig`, `dir`, `ping`, `tasklist`.
- Other programs: PHP, Git, FFmpeg, PostgreSQL utilities, and more.

Imagine you are writing a Python network diagnostic tool. You want Python to execute the Linux `ping` command and collect its output.

You can use `subprocess` to do this.

```python
import subprocess

result = subprocess.run(["ping", "-c", "1", "127.0.0.1"])

print(result)
```

### How does it work?

1. Python receives the command.
2. Python starts an external process.
3. The operating system executes the program.
4. Python waits for the process to finish when using `run()`.
5. Python receives the process result.

**Important:** A subprocess is a separate operating-system process, not a Python thread.

## 2. Importing subprocess

```python
import subprocess
```

You do not need to install it with pip. It is part of Python's standard library.

Official documentation:

https://docs.python.org/3/library/subprocess.html

---

# 3. subprocess.run()

`subprocess.run()` is the recommended starting point for most subprocess operations.

It executes a command and waits for it to finish.

### Basic syntax

```python
subprocess.run(args, ...)
```

Example:

```python
import subprocess

subprocess.run(["whoami"])
```

The first argument is a list:

```python
["whoami"]
```

The first element is the program to execute. The remaining elements are its arguments.

### Example with arguments

Linux:

```python
subprocess.run(["ping", "-c", "2", "127.0.0.1"])
```

Windows:

```python
subprocess.run(["ping", "-n", "2", "127.0.0.1"])
```

Notice that Linux and Windows use different options for specifying the number of ping requests.

## 4. All important subprocess.run() parameters

| Parameter | Meaning |
|---|---|
| `args` | Command and arguments to execute |
| `stdin` | Input stream for the process |
| `input` | Data to send to the process |
| `stdout` | Where standard output should go |
| `stderr` | Where standard error should go |
| `capture_output` | Capture stdout and stderr |
| `shell` | Execute through a shell when True |
| `cwd` | Working directory for the process |
| `timeout` | Maximum time to wait |
| `check` | Raise an exception for nonzero exit status |
| `text` | Work with strings instead of bytes |
| `encoding` | Text encoding |
| `errors` | Encoding error handling |
| `env` | Environment variables for the process |
| `universal_newlines` | Older alias for text mode |
| `creationflags` | Windows process creation flags |
| `startupinfo` | Windows startup configuration |
| `close_fds` | Control inheritance of file descriptors |
| `user` | POSIX user identity, where supported |
| `group` | POSIX group identity, where supported |
| `extra_groups` | Additional POSIX groups, where supported |
| `umask` | POSIX file creation mask, where supported |
| `pass_fds` | POSIX file descriptors to preserve |
| `restore_signals` | Restore certain signals to their default behavior |

Not every parameter is available or meaningful on every operating system.

### 4.1 args

The command to execute.

```python
subprocess.run(["pwd"])
```

You can also use a command string with `shell=True`, but prefer an argument list with `shell=False`.

### 4.2 capture_output

Captures both standard output and standard error.

```python
result = subprocess.run(
    ["whoami"],
    capture_output=True
)

print(result.stdout)
print(result.stderr)
```

Without text mode, captured output is bytes.

### 4.3 text

Converts captured output into strings.

```python
result = subprocess.run(
    ["whoami"],
    capture_output=True,
    text=True
)

print(result.stdout)
```

Equivalent older option:

```python
universal_newlines=True
```

Prefer `text=True` in modern Python code.

### 4.4 encoding

Specifies the encoding used to decode output.

```python
result = subprocess.run(
    ["whoami"],
    capture_output=True,
    text=True,
    encoding="utf-8"
)
```

### 4.5 errors

Controls how encoding errors are handled.

```python
result = subprocess.run(
    ["some_command"],
    capture_output=True,
    text=True,
    encoding="utf-8",
    errors="replace"
)
```

Common values:

- `strict`: raise an error for invalid encoded data.
- `replace`: replace invalid characters.
- `ignore`: skip invalid characters.

### 4.6 timeout

Sets the maximum number of seconds to wait.

```python
import subprocess

try:
    result = subprocess.run(
        ["ping", "-c", "10", "127.0.0.1"],
        capture_output=True,
        text=True,
        timeout=3
    )

except subprocess.TimeoutExpired:
    print("The process took too long!")
```

A timeout is especially useful when executing commands that might hang.

### 4.7 check

Controls whether a nonzero exit code raises an exception.

```python
result = subprocess.run(
    ["ls", "/"],
    capture_output=True,
    text=True,
    check=True
)
```

When `check=True`, Python raises:

```python
subprocess.CalledProcessError
```

when the command exits with a nonzero status.

### 4.8 cwd

Specifies the working directory.

```python
result = subprocess.run(
    ["ls"],
    cwd="/home/charles",
    capture_output=True,
    text=True
)
```

This means the command executes as if its working directory were `/home/charles`.

### 4.9 env

Provides environment variables to the subprocess.

```python
import os
import subprocess

new_env = os.environ.copy()

new_env["MY_VARIABLE"] = "Hello Charles"

result = subprocess.run(
    ["python3", "-c", "import os; print(os.getenv('MY_VARIABLE'))"],
    env=new_env,
    capture_output=True,
    text=True
)

print(result.stdout)
```

Use `os.environ.copy()` if you want to preserve existing environment variables and add or modify selected ones.

### 4.10 input

Sends input to the subprocess through standard input.

```python
import subprocess

result = subprocess.run(
    ["python3", "-c", "print(input())"],
    input="Hello from Python",
    capture_output=True,
    text=True
)

print(result.stdout)
```

The external Python process receives the string as standard input.

### 4.11 stdout and stderr

These control where output streams go.

Capture standard output:

```python
result = subprocess.run(
    ["ls"],
    stdout=subprocess.PIPE,
    text=True
)

print(result.stdout)
```

Send standard output to a file:

```python
with open("output.txt", "w", encoding="utf-8") as file:
    subprocess.run(
        ["ls", "-la"],
        stdout=file,
        text=True
    )
```

Discard output:

```python
subprocess.run(
    ["ping", "-c", "1", "127.0.0.1"],
    stdout=subprocess.DEVNULL,
    stderr=subprocess.DEVNULL
)
```

Capture errors separately:

```python
result = subprocess.run(
    ["ls", "/nonexistent"],
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE,
    text=True
)

print("OUTPUT:", result.stdout)
print("ERROR:", result.stderr)
```

Merge errors into standard output:

```python
result = subprocess.run(
    ["ls", "/"],
    stdout=subprocess.PIPE,
    stderr=subprocess.STDOUT,
    text=True
)

print(result.stdout)
```

---

# 5. Understanding CompletedProcess

When you use `subprocess.run()`, Python returns a `CompletedProcess` object.

```python
result = subprocess.run(
    ["whoami"],
    capture_output=True,
    text=True
)
```

The important attributes are:

| Attribute | Meaning |
|---|---|
| `args` | Command that was executed |
| `returncode` | Process exit status |
| `stdout` | Captured standard output |
| `stderr` | Captured standard error |

Example:

```python
print(result.args)
print(result.returncode)
print(result.stdout)
print(result.stderr)
```

### Understanding returncode

Normally:

- `0`: successful execution.
- Nonzero: command reported a failure.
- On POSIX, a negative return code can indicate termination by a signal.

The exact meaning of positive exit codes depends on the program.

Example:

```python
if result.returncode == 0:
    print("Command successful")

else:
    print("Command failed")
```

---

# 6. subprocess exceptions

The main exceptions to understand are:

| Exception | Meaning |
|---|---|
| `CalledProcessError` | Command returned a nonzero status when `check=True` |
| `TimeoutExpired` | Process exceeded the timeout |
| `FileNotFoundError` | Executable could not be found |
| `PermissionError` | Insufficient permission to execute the program |
| `OSError` | Other operating-system execution errors |

### Complete error-handling example

```python
import subprocess

try:
    result = subprocess.run(
        ["ping", "-c", "1", "127.0.0.1"],
        capture_output=True,
        text=True,
        timeout=5,
        check=True
    )

    print(result.stdout)

except subprocess.CalledProcessError as error:
    print("Command failed:", error.returncode)
    print(error.stderr)

except subprocess.TimeoutExpired:
    print("Command timed out")

except FileNotFoundError:
    print("The executable was not found")

except PermissionError:
    print("Permission denied")

except OSError as error:
    print("Operating-system error:", error)
```

---

# 7. subprocess.PIPE

`PIPE` creates a communication channel between processes or between Python and a process.

```python
subprocess.PIPE
```

It is commonly used with:

- `stdout`
- `stderr`
- `stdin`

Example:

```python
result = subprocess.run(
    ["whoami"],
    stdout=subprocess.PIPE,
    text=True
)

print(result.stdout)
```

Modern equivalent for capturing both streams:

```python
result = subprocess.run(
    ["whoami"],
    capture_output=True,
    text=True
)
```

Remember:

`capture_output=True` is a convenient shortcut for capturing both `stdout` and `stderr`.

Do not combine `capture_output=True` with explicit `stdout=` or `stderr=` arguments.

---

# 8. subprocess.DEVNULL

`DEVNULL` discards output.

```python
subprocess.run(
    ["ping", "-c", "1", "127.0.0.1"],
    stdout=subprocess.DEVNULL,
    stderr=subprocess.DEVNULL
)
```

Use it when you do not need the output.

---

# 9. subprocess.Popen()

`Popen` gives you greater control over a process.

Unlike `run()`, it starts the process and returns immediately, allowing your program to interact with it while it is running.

### Basic syntax

```python
process = subprocess.Popen(args, ...)
```

Example:

```python
import subprocess

process = subprocess.Popen(
    ["ping", "-c", "4", "127.0.0.1"]
)

print("Process started")
```

Python does not automatically wait for the process to finish here.

## 10. Popen parameters

Many parameters are shared with `subprocess.run()`:

| Parameter | Meaning |
|---|---|
| `args` | Command and arguments |
| `bufsize` | Buffering configuration |
| `executable` | Alternative executable |
| `stdin` | Standard input |
| `stdout` | Standard output |
| `stderr` | Standard error |
| `preexec_fn` | POSIX function executed before the child starts |
| `close_fds` | File descriptor inheritance |
| `shell` | Whether to use a shell |
| `cwd` | Working directory |
| `env` | Environment variables |
| `universal_newlines` | Text mode |
| `startupinfo` | Windows startup configuration |
| `creationflags` | Windows creation flags |
| `restore_signals` | Restore certain signal handlers |
| `start_new_session` | Start a new POSIX session |
| `pass_fds` | Preserve selected POSIX file descriptors |
| `user` | POSIX user identity, where supported |
| `group` | POSIX group identity, where supported |
| `extra_groups` | Additional POSIX groups |
| `encoding` | Text encoding |
| `errors` | Encoding error handling |
| `text` | Text mode |
| `umask` | POSIX file creation mask |
| `pipesize` | Pipe buffer size where supported |
| `process_group` | POSIX process group configuration |

Some parameters are platform-specific. Avoid `preexec_fn` in threaded applications because it can cause deadlocks.

---

# 11. Popen properties and attributes

After creating a process:

```python
process = subprocess.Popen(["sleep", "5"])
```

You can inspect its properties.

| Attribute | Meaning |
|---|---|
| `args` | Command arguments |
| `pid` | Process ID |
| `returncode` | Exit status; initially `None` while running |
| `stdin` | Standard input stream, if piped |
| `stdout` | Standard output stream, if piped |
| `stderr` | Standard error stream, if piped |
| `universal_newlines` | Indicates text mode |
| `text_mode` | Indicates whether text mode is enabled |
| `encoding` | Configured encoding |
| `errors` | Configured encoding error policy |

Example:

```python
import subprocess

process = subprocess.Popen(["sleep", "5"])

print("PID:", process.pid)
print("Arguments:", process.args)
print("Return code:", process.returncode)
```

The return code will normally be `None` if the process has not finished yet.

---

# 12. Popen methods

| Method | Purpose |
|---|---|
| `poll()` | Check whether the process has finished |
| `wait()` | Wait for the process to finish |
| `communicate()` | Send input and collect output streams |
| `send_signal()` | Send a signal to the process |
| `terminate()` | Request process termination |
| `kill()` | Forcefully terminate the process |
| `__enter__()` | Support context manager entry |
| `__exit__()` | Support context manager exit |

### 12.1 poll()

Checks whether the process has finished without waiting.

```python
import subprocess
import time

process = subprocess.Popen(["sleep", "3"])

while process.poll() is None:
    print("Still running...")
    time.sleep(1)

print("Finished:", process.returncode)
```

### 12.2 wait()

Waits until the process finishes.

```python
process = subprocess.Popen(["sleep", "3"])

return_code = process.wait()

print(return_code)
```

It returns the process exit status.

You can set a timeout:

```python
process.wait(timeout=5)
```

### 12.3 communicate()

Sends optional input and collects output.

```python
import subprocess

process = subprocess.Popen(
    ["python3", "-c", "print(input())"],
    stdin=subprocess.PIPE,
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE,
    text=True
)

stdout, stderr = process.communicate(
    input="Hello Charles",
    timeout=5
)

print(stdout)
print(stderr)
```

**Important:** Prefer `communicate()` instead of manually reading multiple pipes and then calling `wait()`. This helps prevent pipe-buffer deadlocks.

### 12.4 terminate()

Requests that the process stop.

```python
process = subprocess.Popen(["sleep", "30"])

process.terminate()

process.wait()
```

On POSIX, this normally sends `SIGTERM`. On Windows, it uses the platform's process termination mechanism.

### 12.5 kill()

Forcefully terminates the process.

```python
process = subprocess.Popen(["sleep", "30"])

process.kill()

process.wait()
```

On POSIX, this normally sends `SIGKILL`.

Use `terminate()` first when graceful shutdown is possible. Use `kill()` when necessary.

### 12.6 send_signal()

Sends a signal to the process.

```python
import subprocess
import signal

process = subprocess.Popen(["sleep", "30"])

process.send_signal(signal.SIGTERM)

process.wait()
```

Signals are platform-dependent.

---

# 13. Context manager with Popen

You can use `Popen` with `with`.

```python
import subprocess

with subprocess.Popen(
    ["python3", "-c", "print('Hello')"],
    stdout=subprocess.PIPE,
    text=True
) as process:

    output = process.stdout.read()

    print(output)
```

The context manager helps ensure that the process object and its streams are cleaned up.

For normal execution, `subprocess.run()` is usually simpler.

---

# 14. Shell=True versus Shell=False

This is one of the most important security concepts in subprocess.

### shell=False

```python
subprocess.run(
    ["echo", "Hello Charles"],
    shell=False
)
```

Python executes the program directly.

### shell=True

```python
subprocess.run(
    "echo Hello Charles",
    shell=True
)
```

Python asks the operating system's shell to interpret the command string.

On Linux, this is normally `/bin/sh`.

The shell can interpret special characters such as:

- `;`
- `&&`
- `||`
- `|`
- `>`
- `$()`
- `*`

### Why shell=True can be dangerous

Consider:

```python
import subprocess

username = input("Username: ")

command = f"echo {username}"

subprocess.run(command, shell=True)
```

If untrusted input contains shell syntax, the shell may interpret it as part of the command.

### Recommended approach

```python
import subprocess

username = input("Username: ")

subprocess.run(
    ["echo", username],
    shell=False
)
```

Using an argument list prevents shell interpretation, but it does not automatically make every argument safe. Validate input and consider whether the called program has dangerous options.

---

# 15. Piping commands together

A pipe sends one process's output into another process's input.

Shell example:

```bash
ps aux | grep python
```

Python equivalent:

```python
import subprocess

ps = subprocess.Popen(
    ["ps", "aux"],
    stdout=subprocess.PIPE,
    text=True
)

grep = subprocess.run(
    ["grep", "python"],
    stdin=ps.stdout,
    capture_output=True,
    text=True
)

ps.stdout.close()
ps.wait()

print(grep.stdout)
```

What happens?

1. `ps aux` produces a process list.
2. `stdout=subprocess.PIPE` makes its output available to Python.
3. `grep` receives that output through `stdin`.
4. `grep` filters the lines containing `python`.
5. Python prints the filtered output.

This is useful when you want to combine commands without using `shell=True`.

For more complex pipelines, check every process's return code and handle failures.

---

# 16. Security best practices for subprocess

1. Prefer `subprocess.run()` for ordinary commands.
2. Use an argument list.
3. Keep `shell=False`.
4. Validate all untrusted arguments.
5. Use a timeout.
6. Avoid running commands as root or administrator unnecessarily.
7. Capture output only when needed.
8. Do not place passwords, API keys or tokens in command-line arguments.
9. Handle exceptions.
10. Use absolute executable paths when executable selection is security-sensitive.
11. Control the environment with `env` when appropriate.
12. Be careful with inherited environment variables and file descriptors.
13. Do not trust a successful process exit code as proof that its output is correct.

### Example: safe Linux ping utility

```python
import subprocess
import ipaddress

def ping_host(host: str) -> bool:

    try:
        ip = ipaddress.ip_address(host)

        result = subprocess.run(
            ["ping", "-c", "1", str(ip)],
            capture_output=True,
            text=True,
            timeout=5,
            check=False
        )

        if result.returncode == 0:
            print(result.stdout)
            return True

        print(result.stderr)
        return False

    except ValueError:
        print("Invalid IP address")

    except subprocess.TimeoutExpired:
        print("Ping timed out")

    except FileNotFoundError:
        print("Ping executable not found")

    return False


ping_host("127.0.0.1")
```

This is a useful foundation for network diagnostic tools.

---

# PART 2: THE THREADING MODULE

## 17. What is threading?

Threading allows a Python program to execute multiple threads within the same process.

A thread is a separate path of execution within a process.

Imagine downloading three files.

Without threading:

1. Download file A.
2. Wait until A finishes.
3. Download file B.
4. Wait until B finishes.
5. Download file C.

With threading, the three downloads can make progress concurrently.

### Process versus thread

| Process | Thread |
|---|---|
| Has its own process memory space | Shares process memory |
| More isolated | Less isolated |
| Communicates through IPC or other mechanisms | Can share variables directly |
| Usually has more creation overhead | Usually has less creation overhead |
| Can run independently | Exists inside a process |

Threads are particularly useful for I/O-bound tasks such as:

- Network requests.
- File operations.
- Waiting for external commands.
- Database operations.
- Reading and writing data.

### Important: the GIL

In conventional GIL-enabled CPython builds, only one thread at a time executes Python bytecode.

This means threading does not normally make CPU-bound Python calculations execute in parallel.

For CPU-bound work, consider:

- `multiprocessing`
- `ProcessPoolExecutor`
- Free-threaded Python builds, where appropriate and supported.

---

# 18. Importing threading

```python
import threading
```

It is included in Python's standard library.

Official documentation:

https://docs.python.org/3/library/threading.html

---

# 19. Creating a thread

The most common way is to create a `Thread` object.

```python
import threading

def worker():
    print("Worker is running")

thread = threading.Thread(target=worker)

thread.start()
thread.join()

print("Main program finished")
```

### Understanding the process

1. `worker()` defines the task.
2. `Thread()` creates a thread object.
3. `target=worker` tells the thread which function to execute.
4. `start()` starts the thread.
5. `join()` makes the main thread wait for it.
6. The main program continues after the worker finishes.

**Important:** Write `target=worker`, not `target=worker()`.

- `worker` passes the function to the thread.
- `worker()` executes the function immediately.

---

# 20. Thread constructor parameters

The main `threading.Thread` parameters are:

| Parameter | Meaning |
|---|---|
| `group` | Reserved for future extension; must be `None` |
| `target` | Function to execute |
| `name` | Name of the thread |
| `args` | Positional arguments for the target |
| `kwargs` | Keyword arguments for the target |
| `daemon` | Whether the thread is a daemon thread |
| `context` | Context variable context in Python versions that support it |

### 20.1 target

The function to execute.

```python
def show():
    print("Hello")

thread = threading.Thread(target=show)
```

### 20.2 name

Gives a thread a custom name.

```python
thread = threading.Thread(
    target=show,
    name="Worker-1"
)
```

### 20.3 args

Passes positional arguments.

```python
import threading

def greet(name):
    print(f"Hello {name}")

thread = threading.Thread(
    target=greet,
    args=("Charles",)
)

thread.start()
thread.join()
```

Notice the comma:

```python
args=("Charles",)
```

This creates a one-element tuple.

### 20.4 kwargs

Passes keyword arguments.

```python
def greet(name, age):
    print(name, age)

thread = threading.Thread(
    target=greet,
    kwargs={
        "name": "Charles",
        "age": 25
    }
)

thread.start()
thread.join()
```

### 20.5 daemon

Controls whether a thread is a daemon thread.

```python
thread = threading.Thread(
    target=show,
    daemon=True
)
```

A daemon thread does not keep the Python process alive when only daemon threads remain.

Be careful: daemon threads can be stopped abruptly when the interpreter shuts down. Use non-daemon threads and an explicit shutdown mechanism when cleanup matters.

### 20.6 context

Newer Python versions support a `context` argument for controlling context variable propagation.

The exact default behavior depends on the Python build and version.

Check your installed version's documentation before relying on this argument.

---

# 21. Thread properties and attributes

After creating a thread:

```python
thread = threading.Thread(target=show, name="Worker")
```

Important attributes and properties include:

| Attribute/property | Meaning |
|---|---|
| `name` | Thread name |
| `ident` | Thread identifier, or `None` before it starts |
| `native_id` | Operating-system thread ID, or `None` before it starts |
| `daemon` | Whether it is a daemon thread |
| `is_alive()` | Method that checks whether the thread is alive |
| `target` | Target function stored internally |
| `args` | Target positional arguments stored internally |
| `kwargs` | Target keyword arguments stored internally |

**Note:** `target`, `args`, and `kwargs` are implementation attributes, not the main public interface for managing threads. Do not rely on them after the thread has started.

---

# 22. Thread methods

| Method | Purpose |
|---|---|
| `start()` | Start the thread |
| `run()` | Execute the thread's target function |
| `join()` | Wait for the thread to finish |
| `is_alive()` | Check whether the thread is running |
| `getName()` | Deprecated older name getter |
| `setName()` | Deprecated older name setter |
| `isDaemon()` | Deprecated older daemon getter |
| `setDaemon()` | Deprecated older daemon setter |

Prefer properties such as `name` and `daemon`, rather than the deprecated methods.

### 22.1 start()

```python
thread.start()
```

Starts the thread and invokes its `run()` method in the new thread.

You cannot start the same `Thread` object twice.

### 22.2 run()

```python
thread.run()
```

Calls the target function directly in the current thread if invoked manually.

Normally, use `start()` to get concurrent execution.

### 22.3 join()

```python
thread.join()
```

Waits for the thread to finish.

You can set a timeout:

```python
thread.join(timeout=5)
```

If the timeout expires, the calling thread continues. `join()` returns `None`, so use `is_alive()` to see whether the thread is still running.

### 22.4 is_alive()

```python
if thread.is_alive():
    print("Thread is still running")
else:
    print("Thread has finished")
```

### 22.5 name

You can inspect or change a thread's name.

```python
thread.name = "Network-Worker"

print(thread.name)
```

### 22.6 daemon

```python
thread.daemon = True
```

Set this before starting the thread.

---

# 23. threading.current_thread()

Returns the currently executing thread object.

```python
import threading

def worker():
    current = threading.current_thread()

    print("Thread name:", current.name)
    print("Thread ID:", current.ident)

thread = threading.Thread(target=worker, name="Worker-1")

thread.start()
thread.join()
```

Useful when debugging applications with many threads.

---

# 24. threading.main_thread()

Returns the main thread object.

```python
import threading

main = threading.main_thread()

print(main.name)
print(main.ident)
```

The main thread is where the Python program originally began executing.

---

# 25. threading.active_count()

Returns the number of currently active Thread objects.

```python
import threading

print(threading.active_count())
```

This includes the calling thread.

---

# 26. threading.enumerate()

Returns a list of currently active Thread objects.

```python
import threading

for thread in threading.enumerate():
    print(thread.name, thread.ident)
```

Useful for debugging and monitoring threads.

---

# 27. threading.Lock()

A lock prevents multiple threads from entering a protected section of code at the same time.

This is important when threads share data.

### The problem: race conditions

```python
import threading

balance = 0

def deposit():
    global balance

    for _ in range(100000):
        balance += 1

threads = [
    threading.Thread(target=deposit)
    for _ in range(2)
]

for thread in threads:
    thread.start()

for thread in threads:
    thread.join()

print(balance)
```

The expected result is 200000, but unsynchronized shared-state updates can produce incorrect results.

This is called a race condition.

### Correct approach using Lock

```python
import threading

balance = 0
lock = threading.Lock()

def deposit():
    global balance

    for _ in range(100000):
        with lock:
            balance += 1

threads = [
    threading.Thread(target=deposit)
    for _ in range(2)
]

for thread in threads:
    thread.start()

for thread in threads:
    thread.join()

print(balance)
```

The `with lock:` statement acquires the lock and releases it automatically when the block finishes.

## 28. Lock methods

| Method | Purpose |
|---|---|
| `acquire()` | Acquire the lock |
| `release()` | Release the lock |
| `locked()` | Check whether the lock is currently locked |
| `__enter__()` | Acquire through a context manager |
| `__exit__()` | Release through a context manager |

### acquire()

```python
lock.acquire()

try:
    print("Protected operation")

finally:
    lock.release()
```

You can also use:

```python
lock.acquire(timeout=3)
```

The return value tells you whether the lock was acquired.

### release()

Releases an acquired lock.

Calling `release()` when the lock is not locked raises `RuntimeError`.

### locked()

```python
print(lock.locked())
```

Returns whether the lock is currently locked.

Prefer:

```python
with lock:
    # Shared operation
    pass
```

for ordinary locking.

---

# 29. threading.RLock()

An `RLock` is a reentrant lock.

Unlike a normal Lock, the same thread can acquire it multiple times.

```python
import threading

lock = threading.RLock()

def first():
    with lock:
        print("First")

        with lock:
            print("Second")

thread = threading.Thread(target=first)

thread.start()
thread.join()
```

A normal `Lock` would block if the same thread tried to acquire it again before releasing it.

Use `RLock` when recursive or nested code genuinely needs to acquire the same lock.

Methods:

- `acquire()`
- `release()`

---

# 30. threading.Event()

An Event allows one thread to signal another thread.

Think of it as a communication flag.

```python
import threading
import time

event = threading.Event()

def worker():
    print("Waiting for signal...")

    event.wait()

    print("Signal received!")

thread = threading.Thread(target=worker)
thread.start()

time.sleep(2)

event.set()

thread.join()
```

### Event methods

| Method | Purpose |
|---|---|
| `set()` | Set the event flag |
| `clear()` | Reset the event flag |
| `wait()` | Wait until the event is set |
| `is_set()` | Check whether the event is set |

### Example: stopping a worker

```python
import threading
import time

stop_event = threading.Event()

def worker():
    while not stop_event.is_set():
        print("Working...")
        time.sleep(1)

    print("Worker stopped")

thread = threading.Thread(target=worker)

thread.start()

time.sleep(3)

stop_event.set()

thread.join()
```

This is a recommended way to request that a thread stop.

Python does not provide a safe general-purpose method to forcibly kill an arbitrary thread.

---

# 31. threading.Condition()

A Condition allows threads to wait for a particular condition and notify one another when something changes.

It is useful for producer-consumer systems.

```python
import threading

condition = threading.Condition()
ready = False

def consumer():
    global ready

    with condition:
        condition.wait_for(lambda: ready)

        print("Data is ready")

def producer():
    global ready

    with condition:
        ready = True

        condition.notify_all()

thread1 = threading.Thread(target=consumer)
thread2 = threading.Thread(target=producer)

thread1.start()
thread2.start()

thread1.join()
thread2.join()
```

### Condition methods

| Method | Purpose |
|---|---|
| `acquire()` | Acquire its underlying lock |
| `release()` | Release its underlying lock |
| `wait()` | Wait for notification |
| `wait_for()` | Wait until a predicate becomes true |
| `notify()` | Wake one waiting thread |
| `notify_all()` | Wake all waiting threads |

Always call `wait()` and `notify()` while holding the associated condition lock.

---

# 32. threading.Semaphore()

A Semaphore controls how many threads can access a resource simultaneously.

Imagine you have only three available connections to a service.

```python
import threading
import time

semaphore = threading.Semaphore(3)

def worker(number):
    with semaphore:
        print(f"Worker {number} entered")

        time.sleep(2)

        print(f"Worker {number} leaving")

threads = []

for number in range(8):
    thread = threading.Thread(
        target=worker,
        args=(number,)
    )

    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()
```

At most three threads can enter the protected section simultaneously.

### Semaphore methods

| Method | Purpose |
|---|---|
| `acquire()` | Acquire a permit |
| `release()` | Return a permit |

### BoundedSemaphore

```python
semaphore = threading.BoundedSemaphore(3)
```

It works like Semaphore, but raises `ValueError` if releases exceed the initial capacity.

Use it to detect accidental over-release.

---

# 33. threading.Barrier()

A Barrier makes a group of threads wait until all participating threads reach the same point.

```python
import threading
import time

barrier = threading.Barrier(3)

def worker(number):
    print(f"Worker {number} is preparing")

    time.sleep(number)

    barrier.wait()

    print(f"Worker {number} continues")

threads = [
    threading.Thread(target=worker, args=(number,))
    for number in range(3)
]

for thread in threads:
    thread.start()

for thread in threads:
    thread.join()
```

All three workers must reach `barrier.wait()` before they continue.

### Barrier properties

| Property | Meaning |
|---|---|
| `parties` | Number of participating threads |
| `n_waiting` | Number currently waiting |
| `broken` | Whether the barrier is broken |

### Barrier methods

| Method | Purpose |
|---|---|
| `wait()` | Wait for all participating threads |
| `reset()` | Reset the barrier |
| `abort()` | Break the barrier |

A timeout can be supplied to `wait()`.

---

# 34. threading.local()

`local()` creates thread-local storage.

Each thread gets its own value, even though the same attribute name is used.

```python
import threading

data = threading.local()

def worker(name):
    data.username = name

    print(
        threading.current_thread().name,
        data.username
    )

threads = []

for name in ["Charles", "Alice", "Bob"]:
    thread = threading.Thread(
        target=worker,
        args=(name,)
    )

    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()
```

Each thread has its own `data.username`.

This is useful when threads need independent temporary state.

---

# 35. threading.Timer()

A Timer executes a function after a specified delay.

```python
import threading

def reminder():
    print("Time is up!")

timer = threading.Timer(5, reminder)

timer.start()
```

It executes `reminder()` after approximately five seconds.

### Timer methods

Timer inherits from Thread, so it supports:

- `start()`
- `join()`
- `is_alive()`
- `cancel()`

### cancel()

```python
timer.cancel()
```

Cancels the timer if its function has not started yet.

### Timer arguments

```python
def greet(name):
    print("Hello", name)

timer = threading.Timer(
    5,
    greet,
    args=("Charles",)
)

timer.start()
```

Timer delays are not exact scheduling guarantees.

---

# 36. threading.excepthook()

When an exception escapes a thread's `run()` method, Python invokes the thread exception hook.

You can customize the hook.

```python
import threading

def custom_hook(args):
    print("Thread:", args.thread.name)
    print("Exception:", args.exc_value)

threading.excepthook = custom_hook

def worker():
    raise ValueError("Something went wrong")

thread = threading.Thread(
    target=worker,
    name="Worker"
)

thread.start()
thread.join()
```

Important attributes of the exception-hook arguments include:

- `exc_type`
- `exc_value`
- `exc_traceback`
- `thread`

Avoid retaining exception or thread references indefinitely inside a custom hook because this can create reference cycles or interfere with finalization.

---

# 37. threading module-level functions

These are useful functions available directly from `threading`.

| Function | Purpose |
|---|---|
| `active_count()` | Count active threads |
| `current_thread()` | Get the current thread |
| `main_thread()` | Get the main thread |
| `enumerate()` | Get active threads |
| `get_ident()` | Get current thread identifier |
| `get_native_id()` | Get operating-system thread ID |
| `settrace()` | Set tracing for threads |
| `setprofile()` | Set profiling for threads |
| `settrace_all_threads()` | Set tracing for currently executing Python threads and future threads, where supported |
| `setprofile_all_threads()` | Set profiling for currently executing Python threads and future threads, where supported |
| `stack_size()` | Get or set the stack size for subsequently created threads |
| `excepthook()` | Default hook for uncaught thread exceptions |

Some newer functions are Python-version-dependent.

---

# 38. Thread safety and race conditions

When several threads access shared data, you must think about synchronization.

### Race condition

A race condition occurs when the result depends on the unpredictable order of operations.

Example:

```python
balance = balance + 1
```

This involves reading, adding and writing. Another thread can interfere with the operation.

### Deadlock

A deadlock happens when threads wait indefinitely for resources held by one another.

Example concept:

- Thread A holds Lock 1 and waits for Lock 2.
- Thread B holds Lock 2 and waits for Lock 1.

Neither can continue.

### How to avoid deadlocks

1. Acquire locks in a consistent order.
2. Keep critical sections small.
3. Avoid unnecessary nested locks.
4. Use timeouts where appropriate.
5. Release locks using context managers.
6. Do not wait for a thread while holding a lock that it needs.

---

# 39. Threading versus multiprocessing versus asyncio

| Feature | threading | multiprocessing | asyncio |
|---|---|---|---|
| Execution unit | Threads | Processes | Coroutines/tasks |
| Best suited for | I/O-bound work | CPU-bound work | Asynchronous I/O |
| Shared memory | Yes, within process | Separate processes by default | Same event-loop process |
| Parallel CPU execution | Limited by GIL in conventional CPython | Yes, across processes | Not automatically |
| Resource overhead | Usually lower than processes | Usually higher | Usually low |
| Typical API | `Thread`, `Lock` | `Process`, `Pool` | `async`, `await` |

### When to use each

**Threading:**

- Downloading files.
- Making multiple network requests.
- Waiting for external commands.
- Reading and writing multiple files.

**Multiprocessing:**

- Image processing.
- Large calculations.
- CPU-intensive encryption workloads.
- Data processing that benefits from multiple CPU cores.

**Asyncio:**

- Many concurrent network connections.
- Asynchronous web clients.
- Applications already built around async I/O.

---

# PART 3: SUBPROCESS + THREADING TOGETHER

## 40. Running multiple commands concurrently

Suppose you want to check several local network addresses.

You can use threading to run several independent subprocess operations concurrently.

```python
import subprocess
import threading

def ping_host(host):
    try:
        result = subprocess.run(
            ["ping", "-c", "1", host],
            capture_output=True,
            text=True,
            timeout=5
        )

        if result.returncode == 0:
            print(f"{host}: reachable")

        else:
            print(f"{host}: no successful reply")

    except subprocess.TimeoutExpired:
        print(f"{host}: timed out")

    except FileNotFoundError:
        print("Ping command not found")

hosts = [
    "127.0.0.1",
    "192.168.1.1",
    "192.168.1.110"
]

threads = []

for host in hosts:
    thread = threading.Thread(
        target=ping_host,
        args=(host,)
    )

    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()

print("All checks completed")
```

Use this only for systems and addresses you own or are authorized to test.

### What is happening?

1. Python creates one thread for each host.
2. Each thread executes its own subprocess.
3. Each subprocess runs the ping command.
4. Threads wait independently for their commands to finish.
5. The main thread waits for all workers using `join()`.

This demonstrates how Python can coordinate multiple external processes.

For a larger number of tasks, use a bounded worker pool rather than creating an unlimited number of threads.

---

# PART 4: IMPORTANT BEST PRACTICES

## 41. Subprocess checklist

- Use `run()` for ordinary command execution.
- Use `Popen()` when you need process-level control.
- Prefer lists of arguments.
- Avoid `shell=True` with untrusted input.
- Validate input.
- Use timeouts.
- Handle exceptions.
- Check exit codes.
- Avoid unnecessary administrator privileges.
- Avoid exposing secrets in command-line arguments.

## 42. Threading checklist

- Use threads mainly for I/O-bound tasks.
- Use `start()` to execute a thread concurrently.
- Use `join()` to wait for completion.
- Use locks to protect shared mutable data.
- Use Events for cooperative shutdown.
- Use Semaphores to limit concurrent access.
- Use Queues for producer-consumer communication.
- Avoid unnecessary shared state.
- Avoid daemon threads when cleanup is important.
- Avoid trying to forcibly kill threads.
- Keep critical sections short.
- Use a thread pool when managing many tasks.

---

# PART 5: WHAT TO LEARN NEXT

A good learning sequence:

1. `subprocess.run()`
2. `CompletedProcess`
3. `subprocess.PIPE`
4. `subprocess.Popen()`
5. `communicate()`, `poll()`, `wait()`
6. Process termination and timeouts
7. `threading.Thread`
8. `start()`, `join()`, `is_alive()`
9. `Lock` and `RLock`
10. `Event`
11. `Condition`
12. `Semaphore` and `BoundedSemaphore`
13. `Barrier`
14. `threading.local()`
15. `Timer`
16. Thread pools and `queue.Queue`
17. Combining threading and subprocess safely

## Official documentation

- Subprocess: https://docs.python.org/3/library/subprocess.html
- Threading: https://docs.python.org/3/library/threading.html
- Queue: https://docs.python.org/3/library/queue.html
- Concurrent futures: https://docs.python.org/3/library/concurrent.futures.html
- Multiprocessing: https://docs.python.org/3/library/multiprocessing.html

**Final lesson:** `subprocess` allows Python to execute external programs. `threading` allows Python to coordinate multiple paths of execution inside a process. When you understand both, you can build more capable network diagnostic tools, file-processing utilities, automation programs, and security-testing tools.
