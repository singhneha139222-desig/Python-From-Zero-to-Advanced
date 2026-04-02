# 📘 07_Advanced_Python.md

## 📌 Introduction

**Advanced Python** mein hum powerful features seekhenge jo production-grade code likhne mein help karte hain:

- 🔄 **Iterators & Generators** - Memory efficient data processing
- 🎨 **Decorators** - Function behavior modification
- 🧵 **Concurrency** - Parallel execution (Threading, Multiprocessing, Asyncio)
- 🔧 **Metaprogramming** - Code that writes code
- ⚡ **Performance Optimization** - Fast Python code

> 💡 **Note:** Ye topics intermediate se advanced level ke hain. Basic Python concepts strong hone chahiye!

---

## 🧠 Core Concepts

### Python's Execution Model

```
Source Code (.py)
       ↓
   Compilation
       ↓
Bytecode (.pyc)
       ↓
Python Virtual Machine (PVM)
       ↓
    Execution
```

### Memory Management

| Concept | Description |
|---------|-------------|
| Reference Counting | Track how many references point to object |
| Garbage Collection | Remove objects with 0 references |
| Memory Pool | Pre-allocated memory for small objects |
| Interning | Cache immutable objects (small ints, strings) |

---

## 💻 Code Examples

## 🔄 ITERATORS AND GENERATORS

### 🟢 Basic: Understanding Iterators

```python
# Every iterable has an iterator
my_list = [1, 2, 3, 4, 5]
iterator = iter(my_list)  # Get iterator

print(next(iterator))  # 1
print(next(iterator))  # 2
print(next(iterator))  # 3

# StopIteration when exhausted
# next(iterator)  # After all elements - raises StopIteration

# For loop internally uses iterator
for item in [1, 2, 3]:
    print(item)
    
# Is equivalent to:
iterator = iter([1, 2, 3])
while True:
    try:
        item = next(iterator)
        print(item)
    except StopIteration:
        break
```

### Custom Iterator Class

```python
class CountDown:
    """Iterator that counts down from n to 1"""
    
    def __init__(self, n):
        self.n = n
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.n <= 0:
            raise StopIteration
        self.n -= 1
        return self.n + 1

# Using the iterator
for num in CountDown(5):
    print(num, end=" ")
# Output: 5 4 3 2 1

# Fibonacci Iterator
class Fibonacci:
    """Iterator for Fibonacci sequence"""
    
    def __init__(self, max_count):
        self.max_count = max_count
        self.count = 0
        self.a, self.b = 0, 1
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.count >= self.max_count:
            raise StopIteration
        
        result = self.a
        self.a, self.b = self.b, self.a + self.b
        self.count += 1
        return result

print(list(Fibonacci(10)))
# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

### 🟡 Intermediate: Generators

```python
# Generator function - uses yield instead of return
def countdown(n):
    """Generator that counts down"""
    while n > 0:
        yield n
        n -= 1

# Using generator
gen = countdown(5)
print(next(gen))  # 5
print(next(gen))  # 4

for num in countdown(3):
    print(num, end=" ")
# Output: 3 2 1

# Generator remembers state between yields
def stateful_generator():
    print("Starting...")
    yield 1
    print("After first yield...")
    yield 2
    print("After second yield...")
    yield 3

gen = stateful_generator()
print(next(gen))  # "Starting...", then 1
print(next(gen))  # "After first yield...", then 2

# Generator expression (memory efficient)
# List comprehension - creates all in memory
list_comp = [x**2 for x in range(1000000)]  # 8MB+ memory

# Generator expression - creates on demand
gen_exp = (x**2 for x in range(1000000))   # ~100 bytes!

import sys
print(f"Generator size: {sys.getsizeof(gen_exp)} bytes")
```

### 🔴 Advanced: Generator Methods

```python
# send() - Send value into generator
def accumulator():
    """Generator that accumulates values"""
    total = 0
    while True:
        value = yield total
        if value is not None:
            total += value

acc = accumulator()
next(acc)        # Prime the generator, returns 0
print(acc.send(10))  # 10
print(acc.send(20))  # 30
print(acc.send(5))   # 35

# throw() - Throw exception into generator
def safe_generator():
    try:
        yield 1
        yield 2
        yield 3
    except ValueError:
        yield "Caught ValueError!"

gen = safe_generator()
print(next(gen))  # 1
print(gen.throw(ValueError))  # "Caught ValueError!"

# close() - Close generator
def closeable():
    try:
        while True:
            yield "data"
    except GeneratorExit:
        print("Generator closed!")

gen = closeable()
next(gen)
gen.close()  # "Generator closed!"

# yield from - Delegate to sub-generator
def sub_generator():
    yield 1
    yield 2

def main_generator():
    yield 'a'
    yield from sub_generator()  # Delegates to sub_generator
    yield 'b'

print(list(main_generator()))  # ['a', 1, 2, 'b']

# Practical: Flatten nested structure
def flatten(nested):
    """Recursively flatten nested iterables"""
    for item in nested:
        if isinstance(item, (list, tuple)):
            yield from flatten(item)
        else:
            yield item

nested = [1, [2, 3, [4, 5]], 6, [7, [8, 9]]]
print(list(flatten(nested)))  # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

---

## 🎨 DECORATORS (Deep Dive)

### 🟢 Basic: Function Decorators

```python
# Decorator is a function that takes a function and returns a function

# Simple decorator
def uppercase(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        return result.upper()
    return wrapper

@uppercase
def greet(name):
    return f"hello, {name}"

print(greet("world"))  # HELLO, WORLD

# Decorator with proper signature preservation
from functools import wraps

def my_decorator(func):
    @wraps(func)  # Preserves function metadata
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@my_decorator
def say_hello():
    """This is the docstring"""
    return "Hello!"

print(say_hello.__name__)  # say_hello (not 'wrapper')
print(say_hello.__doc__)   # This is the docstring
```

### 🟡 Intermediate: Decorators with Arguments

```python
from functools import wraps

# Decorator that accepts arguments
def repeat(times):
    """Decorator to repeat function execution"""
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            results = []
            for _ in range(times):
                results.append(func(*args, **kwargs))
            return results
        return wrapper
    return decorator

@repeat(times=3)
def greet(name):
    return f"Hello, {name}!"

print(greet("Rahul"))
# ['Hello, Rahul!', 'Hello, Rahul!', 'Hello, Rahul!']

# More practical: Retry decorator
import time
import random

def retry(max_attempts=3, delay=1, exceptions=(Exception,)):
    """Retry function on failure"""
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            last_exception = None
            for attempt in range(1, max_attempts + 1):
                try:
                    return func(*args, **kwargs)
                except exceptions as e:
                    last_exception = e
                    print(f"Attempt {attempt} failed: {e}")
                    if attempt < max_attempts:
                        time.sleep(delay)
            raise last_exception
        return wrapper
    return decorator

@retry(max_attempts=3, delay=0.5, exceptions=(ConnectionError,))
def fetch_data():
    if random.random() < 0.7:
        raise ConnectionError("Network error")
    return "Data fetched!"

# Caching decorator
def memoize(func):
    """Cache function results"""
    cache = {}
    
    @wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]
    
    wrapper.cache = cache  # Expose cache
    return wrapper

@memoize
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(100))  # Fast due to memoization
print(fibonacci.cache)  # View cached values
```

### 🔴 Advanced: Class Decorators

```python
# Class as decorator
class CountCalls:
    """Decorator that counts function calls"""
    
    def __init__(self, func):
        self.func = func
        self.count = 0
        wraps(func)(self)
    
    def __call__(self, *args, **kwargs):
        self.count += 1
        print(f"Call {self.count} of {self.func.__name__}")
        return self.func(*args, **kwargs)

@CountCalls
def say_hello():
    return "Hello!"

say_hello()  # Call 1 of say_hello
say_hello()  # Call 2 of say_hello
print(say_hello.count)  # 2

# Decorator for classes
def singleton(cls):
    """Make a class a singleton"""
    instances = {}
    
    @wraps(cls)
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    
    return get_instance

@singleton
class Database:
    def __init__(self):
        print("Database initialized")

db1 = Database()  # "Database initialized"
db2 = Database()  # Nothing printed
print(db1 is db2)  # True

# Registering classes with decorator
class PluginRegistry:
    plugins = {}
    
    @classmethod
    def register(cls, name):
        def decorator(plugin_cls):
            cls.plugins[name] = plugin_cls
            return plugin_cls
        return decorator

@PluginRegistry.register("json")
class JSONPlugin:
    def parse(self, data):
        return f"Parsing JSON: {data}"

@PluginRegistry.register("xml")
class XMLPlugin:
    def parse(self, data):
        return f"Parsing XML: {data}"

print(PluginRegistry.plugins)
# {'json': <class 'JSONPlugin'>, 'xml': <class 'XMLPlugin'>}
```

### Chaining Decorators

```python
def bold(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return f"<b>{func(*args, **kwargs)}</b>"
    return wrapper

def italic(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return f"<i>{func(*args, **kwargs)}</i>"
    return wrapper

@bold
@italic
def greet(name):
    return f"Hello, {name}"

print(greet("World"))
# <b><i>Hello, World</i></b>

# Decorators apply bottom-up:
# greet = bold(italic(greet))
```

---

## 🧵 CONCURRENCY AND PARALLELISM

### Understanding GIL (Global Interpreter Lock)

```python
"""
GIL = Global Interpreter Lock

- Only one thread can execute Python bytecode at a time
- CPU-bound tasks: Use multiprocessing (multiple processes)
- I/O-bound tasks: Use threading or asyncio (waiting doesn't hold GIL)

       Task Type         |    Best Solution
-------------------------|-------------------
 CPU-bound (calculations) |  multiprocessing
 I/O-bound (network, file)|  threading / asyncio
 Mixed                    |  combination
"""
```

### 🟢 Threading (I/O-bound tasks)

```python
import threading
import time
from concurrent.futures import ThreadPoolExecutor

# Basic threading
def download_file(filename):
    print(f"Starting download: {filename}")
    time.sleep(2)  # Simulate I/O operation
    print(f"Finished download: {filename}")
    return f"{filename} downloaded"

# Create and start threads
threads = []
for i in range(3):
    t = threading.Thread(target=download_file, args=(f"file_{i}.txt",))
    threads.append(t)
    t.start()

# Wait for all threads to complete
for t in threads:
    t.join()

print("All downloads complete!")

# Better way: ThreadPoolExecutor
def download_file(url):
    time.sleep(1)  # Simulate download
    return f"Downloaded {url}"

urls = ["http://example.com/1", "http://example.com/2", "http://example.com/3"]

with ThreadPoolExecutor(max_workers=3) as executor:
    results = executor.map(download_file, urls)
    for result in results:
        print(result)

# ThreadPoolExecutor with submit for individual tasks
with ThreadPoolExecutor(max_workers=3) as executor:
    futures = [executor.submit(download_file, url) for url in urls]
    for future in futures:
        print(future.result())  # Blocks until result available
```

### Thread Synchronization

```python
import threading

# Problem: Race condition
counter = 0

def increment():
    global counter
    for _ in range(100000):
        counter += 1  # Not thread-safe!

threads = [threading.Thread(target=increment) for _ in range(2)]
for t in threads:
    t.start()
for t in threads:
    t.join()

print(counter)  # Expected: 200000, Actual: varies!

# Solution: Lock
counter = 0
lock = threading.Lock()

def safe_increment():
    global counter
    for _ in range(100000):
        with lock:  # Thread-safe
            counter += 1

threads = [threading.Thread(target=safe_increment) for _ in range(2)]
for t in threads:
    t.start()
for t in threads:
    t.join()

print(counter)  # Always 200000

# Other synchronization primitives
# RLock - Reentrant lock (same thread can acquire multiple times)
# Semaphore - Allow N threads
# Event - Thread signaling
# Condition - Wait for specific condition
```

### 🟡 Multiprocessing (CPU-bound tasks)

```python
import multiprocessing as mp
from concurrent.futures import ProcessPoolExecutor
import time

# CPU-intensive function
def cpu_intensive(n):
    """Calculate sum of squares"""
    return sum(i * i for i in range(n))

# Sequential execution
start = time.time()
results = [cpu_intensive(10000000) for _ in range(4)]
print(f"Sequential: {time.time() - start:.2f}s")

# Parallel execution with ProcessPoolExecutor
start = time.time()
with ProcessPoolExecutor(max_workers=4) as executor:
    results = list(executor.map(cpu_intensive, [10000000] * 4))
print(f"Parallel: {time.time() - start:.2f}s")

# Using multiprocessing.Pool
def square(x):
    return x * x

with mp.Pool(processes=4) as pool:
    results = pool.map(square, range(10))
    print(results)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Shared memory for processes
counter = mp.Value('i', 0)  # Shared integer
lock = mp.Lock()

def increment_shared(counter, lock):
    for _ in range(1000):
        with lock:
            counter.value += 1

processes = [
    mp.Process(target=increment_shared, args=(counter, lock))
    for _ in range(4)
]
for p in processes:
    p.start()
for p in processes:
    p.join()

print(counter.value)  # 4000
```

### 🔴 Asyncio (Asynchronous I/O)

```python
import asyncio
import aiohttp  # pip install aiohttp
import time

# Basic async/await
async def say_hello(delay, name):
    await asyncio.sleep(delay)  # Non-blocking sleep
    return f"Hello, {name}!"

async def main():
    # Sequential
    start = time.time()
    result1 = await say_hello(1, "Rahul")
    result2 = await say_hello(1, "Priya")
    print(f"Sequential: {time.time() - start:.2f}s")
    
    # Concurrent with gather
    start = time.time()
    results = await asyncio.gather(
        say_hello(1, "Rahul"),
        say_hello(1, "Priya"),
        say_hello(1, "Amit")
    )
    print(f"Concurrent: {time.time() - start:.2f}s")
    print(results)

asyncio.run(main())
# Sequential: ~2s
# Concurrent: ~1s

# Async HTTP requests
async def fetch_url(session, url):
    async with session.get(url) as response:
        return await response.text()

async def fetch_all_urls(urls):
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_url(session, url) for url in urls]
        return await asyncio.gather(*tasks)

# Usage:
# urls = ["http://example.com/1", "http://example.com/2"]
# results = asyncio.run(fetch_all_urls(urls))

# Async generator
async def async_range(n):
    for i in range(n):
        await asyncio.sleep(0.1)
        yield i

async def consume_async_generator():
    async for num in async_range(5):
        print(num)

asyncio.run(consume_async_generator())

# Asyncio Queue for producer-consumer
async def producer(queue, n):
    for i in range(n):
        await asyncio.sleep(0.1)
        await queue.put(f"item_{i}")
        print(f"Produced: item_{i}")

async def consumer(queue):
    while True:
        item = await queue.get()
        print(f"Consumed: {item}")
        queue.task_done()

async def main():
    queue = asyncio.Queue()
    
    # Start producer and consumer
    producer_task = asyncio.create_task(producer(queue, 5))
    consumer_task = asyncio.create_task(consumer(queue))
    
    # Wait for producer to finish
    await producer_task
    
    # Wait for queue to be processed
    await queue.join()
    
    # Cancel consumer
    consumer_task.cancel()

asyncio.run(main())
```

---

## 🔧 METAPROGRAMMING

### 🟡 Dynamic Attribute Access

```python
class DynamicObject:
    """Object with dynamic attributes"""
    
    def __init__(self, **kwargs):
        for key, value in kwargs.items():
            setattr(self, key, value)
    
    def __getattr__(self, name):
        """Called when attribute not found"""
        return f"Attribute '{name}' not found"
    
    def __setattr__(self, name, value):
        """Called on every attribute set"""
        print(f"Setting {name} = {value}")
        super().__setattr__(name, value)
    
    def __delattr__(self, name):
        """Called on attribute deletion"""
        print(f"Deleting {name}")
        super().__delattr__(name)

obj = DynamicObject(x=10, y=20)
# Setting x = 10
# Setting y = 20

print(obj.x)  # 10
print(obj.z)  # Attribute 'z' not found

# getattr, setattr, hasattr, delattr
class Person:
    def __init__(self, name):
        self.name = name

p = Person("Rahul")
print(getattr(p, 'name'))          # Rahul
print(getattr(p, 'age', 'N/A'))    # N/A (default)
print(hasattr(p, 'name'))          # True
setattr(p, 'age', 25)              # p.age = 25
delattr(p, 'age')                  # del p.age
```

### Type Function and type()

```python
# type() can create classes dynamically

# Normal class definition
class Dog:
    species = "Canine"
    
    def bark(self):
        return "Woof!"

# Equivalent using type()
def bark(self):
    return "Woof!"

Dog = type(
    'Dog',                          # Class name
    (),                             # Base classes
    {'species': 'Canine', 'bark': bark}  # Attributes and methods
)

dog = Dog()
print(dog.species)  # Canine
print(dog.bark())   # Woof!

# With inheritance
Animal = type('Animal', (), {'kingdom': 'Animalia'})
Cat = type('Cat', (Animal,), {'speak': lambda self: 'Meow!'})

cat = Cat()
print(cat.kingdom)  # Animalia
print(cat.speak())  # Meow!
```

### 🔴 Metaclasses

```python
# Metaclass - Class of a class

# Custom metaclass
class MetaLogger(type):
    """Metaclass that logs class creation"""
    
    def __new__(mcs, name, bases, namespace):
        print(f"Creating class: {name}")
        print(f"Bases: {bases}")
        print(f"Namespace: {list(namespace.keys())}")
        
        # Modify class before creation
        namespace['created_by'] = 'MetaLogger'
        
        return super().__new__(mcs, name, bases, namespace)

class MyClass(metaclass=MetaLogger):
    def method(self):
        pass

# Output:
# Creating class: MyClass
# Bases: ()
# Namespace: ['__module__', '__qualname__', 'method']

print(MyClass.created_by)  # MetaLogger

# Practical: Auto-registration metaclass
class PluginMeta(type):
    registry = {}
    
    def __new__(mcs, name, bases, namespace):
        cls = super().__new__(mcs, name, bases, namespace)
        if name != 'Plugin':  # Don't register base class
            mcs.registry[name] = cls
        return cls

class Plugin(metaclass=PluginMeta):
    pass

class JSONPlugin(Plugin):
    def process(self):
        return "Processing JSON"

class XMLPlugin(Plugin):
    def process(self):
        return "Processing XML"

print(PluginMeta.registry)
# {'JSONPlugin': <class 'JSONPlugin'>, 'XMLPlugin': <class 'XMLPlugin'>}

# Validation metaclass
class ValidatedMeta(type):
    """Ensure all methods have docstrings"""
    
    def __new__(mcs, name, bases, namespace):
        for attr_name, attr_value in namespace.items():
            if callable(attr_value) and not attr_name.startswith('_'):
                if not attr_value.__doc__:
                    raise TypeError(
                        f"Method {attr_name} in {name} must have a docstring"
                    )
        return super().__new__(mcs, name, bases, namespace)
```

### __init_subclass__ (Simpler than Metaclass)

```python
# Python 3.6+ - Simpler alternative to metaclass

class Plugin:
    """Base class with automatic registration"""
    _registry = {}
    
    def __init_subclass__(cls, plugin_name=None, **kwargs):
        super().__init_subclass__(**kwargs)
        name = plugin_name or cls.__name__
        cls._registry[name] = cls

class JSONPlugin(Plugin, plugin_name="json"):
    def process(self):
        return "Processing JSON"

class XMLPlugin(Plugin):  # Uses class name
    def process(self):
        return "Processing XML"

print(Plugin._registry)
# {'json': <class 'JSONPlugin'>, 'XMLPlugin': <class 'XMLPlugin'>}
```

---

## ⚡ PERFORMANCE OPTIMIZATION

### 🟢 Profiling Code

```python
import time
import cProfile
import pstats
from functools import wraps

# Simple timing decorator
def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        end = time.perf_counter()
        print(f"{func.__name__}: {end - start:.6f} seconds")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
    return sum(range(1000000))

result = slow_function()
# slow_function: 1.015678 seconds

# cProfile for detailed profiling
def profile_function():
    total = 0
    for i in range(1000000):
        total += i * i
    return total

# Profile and get stats
profiler = cProfile.Profile()
profiler.enable()
profile_function()
profiler.disable()

stats = pstats.Stats(profiler)
stats.sort_stats('cumulative')
stats.print_stats(10)

# Memory profiling (pip install memory-profiler)
# from memory_profiler import profile
# 
# @profile
# def memory_intensive():
#     big_list = [i * i for i in range(1000000)]
#     return sum(big_list)
```

### 🟡 Optimization Techniques

```python
import sys
from collections import deque
import array

# 1. Use generators instead of lists for large data
# Bad
def get_squares_list(n):
    return [x**2 for x in range(n)]

# Good
def get_squares_gen(n):
    return (x**2 for x in range(n))

# 2. Use local variables in loops
# Slow
import math
def compute_slow(numbers):
    return [math.sqrt(x) for x in numbers]

# Fast (local variable lookup is faster)
def compute_fast(numbers):
    sqrt = math.sqrt  # Local reference
    return [sqrt(x) for x in numbers]

# 3. Use appropriate data structures
# Bad - List for membership testing
def check_membership_list(items, target):
    return target in items  # O(n)

# Good - Set for membership testing
def check_membership_set(items, target):
    return target in items  # O(1)

# 4. Use __slots__ for memory optimization
class NormalPoint:
    def __init__(self, x, y):
        self.x = x
        self.y = y

class SlottedPoint:
    __slots__ = ['x', 'y']
    def __init__(self, x, y):
        self.x = x
        self.y = y

print(sys.getsizeof(NormalPoint(1, 2).__dict__))  # ~104 bytes
# SlottedPoint has no __dict__, saves memory

# 5. Use array for homogeneous numeric data
# List - stores Python objects
python_list = [1, 2, 3, 4, 5]

# Array - stores raw values
int_array = array.array('i', [1, 2, 3, 4, 5])

print(sys.getsizeof(python_list))  # ~120 bytes
print(sys.getsizeof(int_array))    # ~92 bytes

# 6. String concatenation
# Bad
result = ""
for i in range(1000):
    result += str(i)

# Good
result = "".join(str(i) for i in range(1000))

# 7. Use collections.deque for queues
# Bad - List as queue
queue = []
queue.append(1)  # O(1)
queue.pop(0)     # O(n) - shifts all elements!

# Good - Deque as queue
queue = deque()
queue.append(1)    # O(1)
queue.popleft()    # O(1)
```

### 🔴 Using Cython/Numba for Speed

```python
# Numba - JIT compilation (pip install numba)
from numba import jit
import numpy as np

# Regular Python
def sum_python(arr):
    total = 0
    for x in arr:
        total += x
    return total

# Numba optimized
@jit(nopython=True)
def sum_numba(arr):
    total = 0
    for x in arr:
        total += x
    return total

# Comparison
arr = np.random.random(10000000)

import time
start = time.time()
sum_python(arr)
print(f"Python: {time.time() - start:.4f}s")

start = time.time()
sum_numba(arr)  # First call includes compilation
print(f"Numba (with compile): {time.time() - start:.4f}s")

start = time.time()
sum_numba(arr)  # Already compiled
print(f"Numba (cached): {time.time() - start:.4f}s")
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Generator Already Exhausted

```python
# Wrong
gen = (x**2 for x in range(5))
print(list(gen))  # [0, 1, 4, 9, 16]
print(list(gen))  # [] - Already exhausted!

# Correct - Create new generator or use list
def get_squares():
    return (x**2 for x in range(5))

print(list(get_squares()))  # [0, 1, 4, 9, 16]
print(list(get_squares()))  # [0, 1, 4, 9, 16]
```

### ❌ Mistake 2: Threading for CPU-bound Tasks

```python
# Wrong - Threading for CPU-bound work (GIL blocks parallel execution)
import threading

def cpu_work():
    return sum(i*i for i in range(10000000))

# This won't be faster due to GIL
threads = [threading.Thread(target=cpu_work) for _ in range(4)]

# Correct - Use multiprocessing for CPU-bound
import multiprocessing as mp

with mp.Pool(4) as pool:
    results = pool.map(cpu_work, range(4))
```

### ❌ Mistake 3: Not Using functools.wraps

```python
# Wrong - Loses function metadata
def my_decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@my_decorator
def my_function():
    """My docstring"""
    pass

print(my_function.__name__)  # wrapper (wrong!)
print(my_function.__doc__)   # None (lost!)

# Correct
from functools import wraps

def my_decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@my_decorator
def my_function():
    """My docstring"""
    pass

print(my_function.__name__)  # my_function
print(my_function.__doc__)   # My docstring
```

---

## 💡 Pro Tips

### 🔥 Tip 1: Use itertools for Efficient Iteration

```python
import itertools

# Chain iterables
combined = itertools.chain([1, 2], [3, 4], [5])
print(list(combined))  # [1, 2, 3, 4, 5]

# Cycle infinitely
colors = itertools.cycle(['red', 'green', 'blue'])
print([next(colors) for _ in range(7)])

# Combinations and permutations
print(list(itertools.combinations('ABC', 2)))
# [('A', 'B'), ('A', 'C'), ('B', 'C')]

# Group by
data = [('a', 1), ('a', 2), ('b', 3), ('b', 4)]
for key, group in itertools.groupby(data, key=lambda x: x[0]):
    print(key, list(group))
```

### 🔥 Tip 2: Use functools for Function Tools

```python
from functools import partial, lru_cache, reduce

# partial - Pre-fill arguments
def power(base, exp):
    return base ** exp

square = partial(power, exp=2)
cube = partial(power, exp=3)
print(square(5))  # 25

# lru_cache - Memoization
@lru_cache(maxsize=128)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print(fibonacci(100))  # Instant due to caching

# reduce - Fold operation
from functools import reduce
product = reduce(lambda a, b: a * b, [1, 2, 3, 4, 5])
print(product)  # 120
```

### 🔥 Tip 3: Context Managers for Resource Management

```python
from contextlib import contextmanager, asynccontextmanager

@contextmanager
def timer(name):
    import time
    start = time.time()
    try:
        yield
    finally:
        print(f"{name}: {time.time() - start:.4f}s")

with timer("Processing"):
    # Some operation
    sum(range(1000000))

# Async context manager
@asynccontextmanager
async def async_timer(name):
    import time
    start = time.time()
    try:
        yield
    finally:
        print(f"{name}: {time.time() - start:.4f}s")
```

---

## 🧪 Practice Questions

### 🟢 Easy

1. Create a generator that yields even numbers up to n.
2. Write a decorator that logs function calls with arguments.
3. Implement a simple iterator for a linked list.

### 🟡 Medium

4. Create a memoization decorator that also tracks cache hits/misses.
5. Implement a thread-safe counter using threading.Lock.
6. Write an async function that fetches multiple URLs concurrently.

### 🔴 Hard

7. Create a metaclass that adds logging to all methods.
8. Implement a coroutine-based event loop from scratch.
9. Build a worker pool using multiprocessing with job queue.

---

## 🚀 Mini Projects

### Project 1: Async Web Scraper

Build an async web scraper that:
- Fetches multiple pages concurrently
- Parses HTML and extracts data
- Handles rate limiting
- Saves results to file

### Project 2: Parallel Data Processor

Create a data processing pipeline that:
- Uses multiprocessing for CPU-bound tasks
- Uses asyncio for I/O-bound tasks
- Implements progress tracking
- Handles errors gracefully

---

## 💼 Interview Insight

### 🔥 Most Asked Questions

1. **Q: Generator vs Iterator difference?**
   **A:** Generator is a simpler way to create iterators using `yield`. Iterator is any object with `__iter__` and `__next__` methods.

2. **Q: GIL kya hai?**
   **A:** Global Interpreter Lock - allows only one thread to execute Python bytecode at a time. Use multiprocessing for CPU-bound tasks.

3. **Q: Decorator kaise kaam karta hai?**
   **A:** Decorator takes a function, wraps it with additional functionality, and returns the wrapper.

4. **Q: asyncio vs threading vs multiprocessing?**
   **A:** 
   - asyncio: Single-threaded, cooperative multitasking for I/O
   - threading: Multiple threads, good for I/O-bound (limited by GIL for CPU)
   - multiprocessing: Multiple processes, bypasses GIL, good for CPU-bound

---

## 📝 Summary

- ✅ Iterators and Generators for memory-efficient iteration
- ✅ Decorators for function/class modification
- ✅ Threading for I/O-bound concurrency
- ✅ Multiprocessing for CPU-bound parallelism
- ✅ Asyncio for async I/O
- ✅ Metaclasses for class customization
- ✅ Performance optimization techniques

---

**Next Module:** [08_Libraries_and_Frameworks.md](./08_Libraries_and_Frameworks.md) - Popular Python Libraries aur Frameworks

---

<div align="center">

**Made with ❤️ for Python Learners**

[⬆️ Back to Top](#-07_advanced_pythonmd)

</div>
