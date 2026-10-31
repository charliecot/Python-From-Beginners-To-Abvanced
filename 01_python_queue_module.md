# Python `queue` Module

## 1. What is a queue?
A queue stores work until a consumer is ready to process it. Python's `queue` module provides thread-safe queues, especially useful for producer/consumer programs and background workers.

```python
from queue import Queue

tasks = Queue()
tasks.put("scan file")
tasks.put("check disk")

print(tasks.get())  # scan file
print(tasks.get())  # check disk
```

## 2. Queue types
```python
from queue import Queue, LifoQueue, PriorityQueue, SimpleQueue
```
- `Queue()`: FIFO — first in, first out.
- `LifoQueue()`: LIFO — last in, first out, like a stack.
- `PriorityQueue()`: returns the lowest-priority value first.
- `SimpleQueue()`: simple unbounded FIFO queue without task tracking methods.

### LIFO
```python
stack = LifoQueue()
stack.put("first")
stack.put("second")
print(stack.get())  # second
```

### Priority queue
Smaller numbers are higher priority:
```python
jobs = PriorityQueue()
jobs.put((3, "low"))
jobs.put((1, "urgent"))
jobs.put((2, "normal"))
print(jobs.get())  # (1, 'urgent')
```
If priorities tie, Python compares later tuple fields. Use comparable fields or a unique sequence number to break ties.

## 3. `put()` and `get()`
- `put(item, block=True, timeout=None)`: add an item.
- `get(block=True, timeout=None)`: retrieve and remove an item.
- `put_nowait(item)`: non-blocking insertion.
- `get_nowait()`: non-blocking retrieval.

`get()` waits when the queue is empty unless you use non-blocking behavior or a timeout.

```python
from queue import Queue, Empty, Full

q = Queue(maxsize=1)
q.put("one")

try:
    q.put_nowait("two")
except Full:
    print("Queue is full")

try:
    print(q.get_nowait())
    print(q.get_nowait())
except Empty:
    print("Queue is empty")
```

A bounded queue applies backpressure so producers cannot keep adding unlimited work.

## 4. `task_done()` and `join()`
For `Queue`, call `task_done()` once for each retrieved task after processing it. `join()` waits until all queued tasks have been marked complete.

```python
from queue import Queue

tasks = Queue()
tasks.put("backup file")
tasks.put("calculate hash")

while not tasks.empty():
    task = tasks.get()
    try:
        print("Processing:", task)
    finally:
        tasks.task_done()

tasks.join()
print("All tasks completed")
```

In concurrent programs, do not rely on `empty()` or `qsize()` for synchronization decisions because another thread may change the queue immediately afterward.

## 5. Producer/consumer worker
```python
from queue import Queue
from threading import Thread

work = Queue()

def worker():
    while True:
        item = work.get()
        try:
            if item is None:  # shutdown signal
                return
            print("Processing:", item)
        finally:
            work.task_done()

thread = Thread(target=worker)
thread.start()

work.put("file-a.txt")
work.put("file-b.txt")
work.join()
work.put(None)  # tell the worker to stop
thread.join()
```
For multiple workers, send one sentinel per worker. Production code should also handle worker failures to avoid waiting forever.

## 6. Thread-safe does not mean everything is thread-safe
Queue operations are designed for communication between threads. Other objects in your program may still require synchronization. For separate processes, consider `multiprocessing.Queue`.

## 7. Common mistakes
- Calling `get()` on an empty queue and not realizing it blocks.
- Forgetting `task_done()` when using `join()`.
- Calling `task_done()` more times than items retrieved.
- Assuming `empty()` or `qsize()` is reliable for concurrency decisions.
- Forgetting to signal worker threads to stop.
- Using `queue.Queue` for inter-process communication.

## 8. Quick reference
| Need | Use |
|---|---|
| FIFO | `Queue()` |
| Stack-like | `LifoQueue()` |
| Priority-based | `PriorityQueue()` |
| Simple FIFO | `SimpleQueue()` |
| Add / retrieve | `put()` / `get()` |
| Non-blocking retrieve | `get_nowait()` |
| Mark task complete | `task_done()` |
| Wait for tracked tasks | `join()` |
| Empty/full exceptions | `Empty` / `Full` |

## 9. Practice
1. Queue five filenames and retrieve them in order.
2. Compare `Queue` and `LifoQueue`.
3. Handle `Full` in a queue of size two.
4. Create a worker that stops on a sentinel.
5. Handle worker exceptions without leaving the program waiting forever.

**Key idea:** a queue separates the code that produces work from the code that processes it.
