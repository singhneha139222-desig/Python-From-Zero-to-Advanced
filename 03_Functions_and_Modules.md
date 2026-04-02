# 📘 03_Functions_and_Modules.md

## 📌 Introduction

**Functions** programming ka backbone hain! Socho ek machine ki tarah - input dalo, process hota hai, output milta hai.

**Real-life analogy:**
- 🧃 **Juicer Machine** = Function
- 🍎 **Fruits** = Input (Arguments)
- 🥤 **Juice** = Output (Return value)

Ek baar juicer bana lo, baar baar use kar sakte ho different fruits ke liye!

**Benefits of Functions:**
- ♻️ **Reusability** - Ek baar likho, baar baar use karo
- 📦 **Modularity** - Code ko manageable pieces mein todo
- 🐛 **Easy Debugging** - Problem isolate karna easy
- 📖 **Readability** - Code samajhna easy

---

## 🧠 Core Concepts

### 1. Function Definition & Calling

```python
# Basic function definition
def greet():
    """This function prints a greeting"""
    print("Hello, World!")

# Calling the function
greet()  # Output: Hello, World!
greet()  # Can call multiple times
```

### 2. Function Anatomy

```python
def function_name(parameters):    # Function header
    """Docstring - describes what function does"""
    # Function body
    result = some_operation
    return result                 # Return statement
```

### 3. Parameters vs Arguments

| Term | Definition | Example |
|------|------------|---------|
| **Parameter** | Variable in function definition | `def greet(name):` - `name` is parameter |
| **Argument** | Actual value passed when calling | `greet("Rahul")` - `"Rahul"` is argument |

---

## 📊 Function Types Table

| Type | Description | Example |
|------|-------------|---------|
| Built-in | Pre-defined in Python | `print()`, `len()`, `type()` |
| User-defined | Created by programmer | `def my_function():` |
| Lambda | Anonymous one-liner | `lambda x: x * 2` |
| Recursive | Calls itself | Factorial, Fibonacci |
| Generator | Yields values | `yield` keyword |
| Higher-order | Takes/returns functions | `map()`, `filter()` |

---

## 💻 Code Examples

### 🟢 Basic Level

#### Functions with Parameters

```python
# Single parameter
def greet(name):
    """Greet a person by name"""
    print(f"Hello, {name}!")

greet("Rahul")      # Output: Hello, Rahul!
greet("Priya")      # Output: Hello, Priya!

# Multiple parameters
def add(a, b):
    """Add two numbers"""
    return a + b

result = add(5, 3)
print(result)       # Output: 8

# Function with no return (returns None)
def print_info(name, age):
    print(f"Name: {name}, Age: {age}")

result = print_info("Rahul", 25)
print(result)       # Output: None
```

#### Default Parameters

```python
def greet(name, greeting="Hello"):
    """Greet with customizable greeting"""
    print(f"{greeting}, {name}!")

greet("Rahul")                  # Output: Hello, Rahul!
greet("Rahul", "Namaste")       # Output: Namaste, Rahul!
greet("Rahul", greeting="Hi")   # Output: Hi, Rahul!

# Default parameter gotcha - mutable default
# ❌ WRONG - Mutable default argument
def append_to(item, lst=[]):
    lst.append(item)
    return lst

print(append_to(1))  # [1]
print(append_to(2))  # [1, 2] - Unexpected!

# ✅ CORRECT - Use None as default
def append_to(item, lst=None):
    if lst is None:
        lst = []
    lst.append(item)
    return lst

print(append_to(1))  # [1]
print(append_to(2))  # [2]
```

#### Return Statement

```python
# Single return value
def square(n):
    return n ** 2

print(square(5))  # Output: 25

# Multiple return values (tuple)
def get_stats(numbers):
    """Return min, max, and average"""
    return min(numbers), max(numbers), sum(numbers) / len(numbers)

minimum, maximum, average = get_stats([1, 2, 3, 4, 5])
print(f"Min: {minimum}, Max: {maximum}, Avg: {average}")
# Output: Min: 1, Max: 5, Avg: 3.0

# Early return
def check_age(age):
    if age < 0:
        return "Invalid age"
    if age < 18:
        return "Minor"
    return "Adult"

print(check_age(-5))   # Output: Invalid age
print(check_age(15))   # Output: Minor
print(check_age(25))   # Output: Adult
```

### 🟡 Intermediate Level

#### Keyword Arguments & Positional Arguments

```python
def describe_person(name, age, city):
    """Describe a person"""
    print(f"{name} is {age} years old from {city}")

# Positional arguments (order matters)
describe_person("Rahul", 25, "Delhi")

# Keyword arguments (order doesn't matter)
describe_person(city="Mumbai", name="Priya", age=23)

# Mixed (positional first, then keyword)
describe_person("Amit", city="Pune", age=27)

# ❌ This will error - positional after keyword
# describe_person(name="Rahul", 25, "Delhi")
```

#### *args and **kwargs

```python
# *args - Variable positional arguments
def add_all(*args):
    """Add any number of arguments"""
    print(f"Args: {args}")  # Tuple
    return sum(args)

print(add_all(1, 2, 3))        # Output: 6
print(add_all(1, 2, 3, 4, 5))  # Output: 15

# **kwargs - Variable keyword arguments
def print_info(**kwargs):
    """Print any keyword arguments"""
    print(f"Kwargs: {kwargs}")  # Dictionary
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Rahul", age=25, city="Delhi")
# Output:
# Kwargs: {'name': 'Rahul', 'age': 25, 'city': 'Delhi'}
# name: Rahul
# age: 25
# city: Delhi

# Combining all parameter types
def full_function(a, b, *args, option=True, **kwargs):
    """Function with all parameter types"""
    print(f"a: {a}, b: {b}")
    print(f"args: {args}")
    print(f"option: {option}")
    print(f"kwargs: {kwargs}")

full_function(1, 2, 3, 4, 5, option=False, name="Test", value=100)
# Output:
# a: 1, b: 2
# args: (3, 4, 5)
# option: False
# kwargs: {'name': 'Test', 'value': 100}
```

#### Lambda Functions (Anonymous Functions)

```python
# Regular function
def square(x):
    return x ** 2

# Equivalent lambda
square_lambda = lambda x: x ** 2

print(square(5))         # Output: 25
print(square_lambda(5))  # Output: 25

# Lambda with multiple arguments
add = lambda a, b: a + b
print(add(3, 4))  # Output: 7

# Lambda with conditional
is_even = lambda x: "Even" if x % 2 == 0 else "Odd"
print(is_even(4))  # Output: Even
print(is_even(5))  # Output: Odd

# Common use: with map, filter, sorted
numbers = [1, 2, 3, 4, 5]

# map - Apply function to all elements
squared = list(map(lambda x: x ** 2, numbers))
print(squared)  # [1, 4, 9, 16, 25]

# filter - Keep elements where function returns True
evens = list(filter(lambda x: x % 2 == 0, numbers))
print(evens)  # [2, 4]

# sorted with custom key
students = [("Rahul", 85), ("Priya", 92), ("Amit", 78)]
sorted_by_score = sorted(students, key=lambda x: x[1], reverse=True)
print(sorted_by_score)  # [('Priya', 92), ('Rahul', 85), ('Amit', 78)]
```

#### Scope and LEGB Rule

```python
# LEGB: Local → Enclosing → Global → Built-in

# Global scope
global_var = "I am global"

def outer():
    # Enclosing scope
    enclosing_var = "I am enclosing"
    
    def inner():
        # Local scope
        local_var = "I am local"
        print(local_var)      # Local
        print(enclosing_var)  # Enclosing
        print(global_var)     # Global
        print(len)            # Built-in
    
    inner()

outer()

# Modifying global variable
counter = 0

def increment():
    global counter  # Declare as global to modify
    counter += 1

increment()
print(counter)  # Output: 1

# nonlocal keyword (for enclosing scope)
def outer():
    count = 0
    
    def inner():
        nonlocal count  # Access enclosing scope variable
        count += 1
        return count
    
    return inner

counter = outer()
print(counter())  # 1
print(counter())  # 2
print(counter())  # 3
```

### 🔴 Advanced Level

#### Closures

```python
# Closure - Function that remembers the environment it was created in

def make_multiplier(factor):
    """Create a multiplier function"""
    def multiplier(number):
        return number * factor  # 'factor' is remembered (enclosed)
    return multiplier

double = make_multiplier(2)
triple = make_multiplier(3)

print(double(5))  # Output: 10
print(triple(5))  # Output: 15

# Practical example: Counter
def make_counter():
    count = 0
    
    def counter():
        nonlocal count
        count += 1
        return count
    
    return counter

counter1 = make_counter()
counter2 = make_counter()

print(counter1())  # 1
print(counter1())  # 2
print(counter2())  # 1 (independent counter)
```

#### Decorators

```python
# Decorator - A function that modifies another function

# Basic decorator
def my_decorator(func):
    def wrapper():
        print("Before function call")
        func()
        print("After function call")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# Output:
# Before function call
# Hello!
# After function call

# Decorator with arguments
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Finished {func.__name__}")
        return result
    return wrapper

@my_decorator
def add(a, b):
    return a + b

print(add(3, 5))
# Output:
# Calling add
# Finished add
# 8

# Practical decorator: Timer
import time

def timer(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.4f} seconds")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
    return "Done"

slow_function()
# Output: slow_function took 1.0012 seconds

# Decorator with parameters
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(times=3)
def greet(name):
    print(f"Hello, {name}!")

greet("Rahul")
# Output:
# Hello, Rahul!
# Hello, Rahul!
# Hello, Rahul!
```

#### Recursion

```python
# Recursion - Function calling itself

# Factorial: n! = n * (n-1)!
def factorial(n):
    """Calculate factorial recursively"""
    if n <= 1:  # Base case
        return 1
    return n * factorial(n - 1)  # Recursive case

print(factorial(5))  # 120 (5 * 4 * 3 * 2 * 1)

# Fibonacci: F(n) = F(n-1) + F(n-2)
def fibonacci(n):
    """Get nth Fibonacci number"""
    if n <= 1:  # Base cases
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print([fibonacci(i) for i in range(10)])
# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]

# ⚠️ Problem: Naive recursion is slow (exponential time)
# Solution: Memoization

from functools import lru_cache

@lru_cache(maxsize=None)
def fibonacci_fast(n):
    """Memoized Fibonacci - O(n) time"""
    if n <= 1:
        return n
    return fibonacci_fast(n - 1) + fibonacci_fast(n - 2)

print(fibonacci_fast(50))  # Fast even for large n

# Tail recursion example (Python doesn't optimize this, but good to know)
def factorial_tail(n, accumulator=1):
    if n <= 1:
        return accumulator
    return factorial_tail(n - 1, n * accumulator)
```

#### Generators

```python
# Generator - Function that yields values one at a time

def count_up_to(n):
    """Generator that counts from 1 to n"""
    i = 1
    while i <= n:
        yield i
        i += 1

# Using generator
counter = count_up_to(5)
print(next(counter))  # 1
print(next(counter))  # 2
print(next(counter))  # 3

# In a loop
for num in count_up_to(5):
    print(num, end=" ")
# Output: 1 2 3 4 5

# Generator expression (like list comprehension but lazy)
gen = (x ** 2 for x in range(1000000))  # No memory allocated yet
print(next(gen))  # 0 - computed on demand
print(next(gen))  # 1

# Practical: Reading large files
def read_large_file(file_path):
    """Memory-efficient file reading"""
    with open(file_path, 'r') as file:
        for line in file:
            yield line.strip()

# Infinite generator
def infinite_counter():
    n = 0
    while True:
        yield n
        n += 1

# Generator with send() and close()
def echo():
    while True:
        received = yield
        print(f"Received: {received}")

gen = echo()
next(gen)  # Prime the generator
gen.send("Hello")  # Output: Received: Hello
gen.send("World")  # Output: Received: World
gen.close()
```

#### First-Class Functions & Higher-Order Functions

```python
# First-class: Functions are objects, can be assigned, passed, returned

# Assign to variable
def greet(name):
    return f"Hello, {name}!"

say_hello = greet
print(say_hello("Rahul"))  # Output: Hello, Rahul!

# Pass as argument
def apply_operation(func, value):
    return func(value)

print(apply_operation(len, "Python"))  # Output: 6
print(apply_operation(str.upper, "hello"))  # Output: HELLO

# Return function
def get_operation(operation):
    operations = {
        'add': lambda a, b: a + b,
        'subtract': lambda a, b: a - b,
        'multiply': lambda a, b: a * b
    }
    return operations.get(operation, lambda a, b: None)

add_func = get_operation('add')
print(add_func(3, 5))  # Output: 8

# Higher-order functions: map, filter, reduce
from functools import reduce

numbers = [1, 2, 3, 4, 5]

# map: Apply function to each element
doubled = list(map(lambda x: x * 2, numbers))
print(doubled)  # [2, 4, 6, 8, 10]

# filter: Keep elements where function is True
evens = list(filter(lambda x: x % 2 == 0, numbers))
print(evens)  # [2, 4]

# reduce: Accumulate values
total = reduce(lambda acc, x: acc + x, numbers, 0)
print(total)  # 15
```

---

## 🧩 Modules and Packages

### Importing Modules

```python
# Import entire module
import math
print(math.pi)        # 3.141592653589793
print(math.sqrt(16))  # 4.0

# Import with alias
import numpy as np
import pandas as pd

# Import specific items
from math import pi, sqrt
print(pi)       # 3.141592653589793
print(sqrt(16)) # 4.0

# Import all (avoid in production)
from math import *
print(sin(0))  # 0.0

# Import from submodule
from os.path import join, exists
```

### Creating Your Own Module

```python
# my_utils.py
"""My utility functions module"""

PI = 3.14159

def greet(name):
    """Greet someone"""
    return f"Hello, {name}!"

def square(n):
    """Square a number"""
    return n ** 2

class Calculator:
    """Simple calculator class"""
    def add(self, a, b):
        return a + b

# In another file:
# import my_utils
# print(my_utils.greet("Rahul"))
# print(my_utils.PI)
```

### Package Structure

```
my_package/
├── __init__.py
├── module1.py
├── module2.py
└── subpackage/
    ├── __init__.py
    └── module3.py
```

```python
# __init__.py can expose module contents
# my_package/__init__.py
from .module1 import function1
from .module2 import function2

# Usage:
# from my_package import function1, function2
```

### `if __name__ == "__main__"`

```python
# my_module.py
def main():
    print("Running as main program")

def helper():
    print("Helper function")

# This block only runs when file is executed directly
# Not when imported as a module
if __name__ == "__main__":
    main()
    
# When imported: only functions are available
# When run directly: main() is called
```

### Standard Library Highlights

```python
# os - Operating system interface
import os
print(os.getcwd())  # Current directory
print(os.listdir('.'))  # List files
os.makedirs('new_folder', exist_ok=True)

# sys - System-specific parameters
import sys
print(sys.version)
print(sys.path)  # Module search paths
# sys.exit(0)  # Exit program

# datetime - Date and time
from datetime import datetime, timedelta
now = datetime.now()
print(now.strftime("%Y-%m-%d %H:%M:%S"))
tomorrow = now + timedelta(days=1)

# random - Random number generation
import random
print(random.randint(1, 100))
print(random.choice(['a', 'b', 'c']))
print(random.shuffle([1, 2, 3, 4, 5]))

# json - JSON encoding/decoding
import json
data = {"name": "Rahul", "age": 25}
json_str = json.dumps(data)
parsed = json.loads(json_str)

# collections - Specialized containers
from collections import Counter, defaultdict, namedtuple
counter = Counter("hello world")
print(counter.most_common(3))  # [('l', 3), ('o', 2), ('h', 1)]

# itertools - Iterator functions
import itertools
print(list(itertools.combinations([1, 2, 3], 2)))
# [(1, 2), (1, 3), (2, 3)]
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Mutable Default Arguments

```python
# Wrong
def add_item(item, lst=[]):
    lst.append(item)
    return lst

print(add_item(1))  # [1]
print(add_item(2))  # [1, 2] - Bug!

# Correct
def add_item(item, lst=None):
    if lst is None:
        lst = []
    lst.append(item)
    return lst
```

### ❌ Mistake 2: Not Returning a Value

```python
# Wrong
def add(a, b):
    result = a + b
    # Forgot return!

x = add(3, 5)
print(x)  # None

# Correct
def add(a, b):
    return a + b
```

### ❌ Mistake 3: Modifying Global Variable Without `global`

```python
# Wrong
count = 0

def increment():
    count += 1  # UnboundLocalError!

# Correct
count = 0

def increment():
    global count
    count += 1
```

### ❌ Mistake 4: Circular Imports

```python
# module_a.py
from module_b import func_b  # Imports module_b

def func_a():
    pass

# module_b.py
from module_a import func_a  # Circular import error!

def func_b():
    pass

# Solution: Import inside function or restructure
```

### ❌ Mistake 5: Lambda for Complex Logic

```python
# Wrong - Hard to read
process = lambda x: x ** 2 if x > 0 else (-x) ** 2 if x < 0 else 0

# Correct - Use regular function
def process(x):
    if x > 0:
        return x ** 2
    elif x < 0:
        return (-x) ** 2
    return 0
```

---

## 💡 Pro Tips (Industry Insights)

### 🔥 Tip 1: Use Type Hints

```python
from typing import List, Dict, Optional, Union, Callable

def greet(name: str) -> str:
    return f"Hello, {name}!"

def process_items(items: List[int]) -> Dict[str, int]:
    return {
        "sum": sum(items),
        "count": len(items)
    }

def find_user(user_id: int) -> Optional[str]:
    # Returns None if not found
    pass

def apply_func(func: Callable[[int], int], value: int) -> int:
    return func(value)
```

### 🔥 Tip 2: Use Docstrings Properly

```python
def calculate_discount(price: float, discount_percent: float) -> float:
    """
    Calculate the discounted price.
    
    Args:
        price: Original price of the item
        discount_percent: Discount percentage (0-100)
    
    Returns:
        The price after applying the discount
    
    Raises:
        ValueError: If discount_percent is not between 0 and 100
    
    Example:
        >>> calculate_discount(100, 20)
        80.0
    """
    if not 0 <= discount_percent <= 100:
        raise ValueError("Discount must be between 0 and 100")
    return price * (1 - discount_percent / 100)
```

### 🔥 Tip 3: Use functools for Function Utilities

```python
from functools import partial, wraps, lru_cache

# partial - Create function with some arguments pre-filled
def power(base, exp):
    return base ** exp

square = partial(power, exp=2)
cube = partial(power, exp=3)
print(square(5))  # 25
print(cube(5))    # 125

# wraps - Preserve function metadata in decorators
def my_decorator(func):
    @wraps(func)  # Preserves __name__, __doc__, etc.
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

# lru_cache - Memoization
@lru_cache(maxsize=128)
def expensive_function(n):
    # Results are cached
    return n ** 2
```

### 🔥 Tip 4: Single Responsibility Principle

```python
# Bad - Function does too many things
def process_user_data(user_data):
    # Validate
    # Clean
    # Save to database
    # Send email
    # Log
    pass

# Good - Each function does one thing
def validate_user(data):
    pass

def clean_user_data(data):
    pass

def save_user(user):
    pass

def send_welcome_email(user):
    pass
```

### 🔥 Tip 5: Use **kwargs for Flexible APIs

```python
def create_user(name, **options):
    """Create user with optional settings"""
    user = {"name": name}
    user.update(options)
    return user

# Flexible usage
user1 = create_user("Rahul")
user2 = create_user("Priya", age=25, city="Mumbai", role="admin")
```

---

## 🔍 Real-world Use Cases

### Use Case 1: Configuration Manager with Decorators

```python
import functools
import time

def retry(max_attempts=3, delay=1):
    """Retry decorator for unreliable functions"""
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            last_exception = None
            for attempt in range(1, max_attempts + 1):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    last_exception = e
                    print(f"Attempt {attempt} failed: {e}")
                    if attempt < max_attempts:
                        time.sleep(delay)
            raise last_exception
        return wrapper
    return decorator

@retry(max_attempts=3, delay=0.5)
def fetch_data(url):
    """Fetch data from URL (might fail)"""
    import random
    if random.random() < 0.7:  # 70% chance of failure
        raise ConnectionError("Network error")
    return "Data fetched successfully!"

# Usage
# result = fetch_data("https://api.example.com")
```

### Use Case 2: Event Handler System

```python
class EventEmitter:
    """Simple event system using callbacks"""
    
    def __init__(self):
        self._events = {}
    
    def on(self, event_name, callback):
        """Register event handler"""
        if event_name not in self._events:
            self._events[event_name] = []
        self._events[event_name].append(callback)
        return self
    
    def emit(self, event_name, *args, **kwargs):
        """Trigger event with data"""
        if event_name in self._events:
            for callback in self._events[event_name]:
                callback(*args, **kwargs)
    
    def off(self, event_name, callback=None):
        """Remove event handler"""
        if callback is None:
            self._events.pop(event_name, None)
        elif event_name in self._events:
            self._events[event_name].remove(callback)

# Usage
emitter = EventEmitter()

def on_user_created(user):
    print(f"User created: {user['name']}")

def send_welcome_email(user):
    print(f"Sending welcome email to {user['email']}")

emitter.on('user_created', on_user_created)
emitter.on('user_created', send_welcome_email)

emitter.emit('user_created', {'name': 'Rahul', 'email': 'rahul@example.com'})
# Output:
# User created: Rahul
# Sending welcome email to rahul@example.com
```

### Use Case 3: Data Pipeline with Generators

```python
def read_csv_rows(filename):
    """Generator to read CSV rows"""
    with open(filename, 'r') as f:
        headers = f.readline().strip().split(',')
        for line in f:
            values = line.strip().split(',')
            yield dict(zip(headers, values))

def filter_active(users):
    """Filter active users"""
    for user in users:
        if user.get('status') == 'active':
            yield user

def transform_user(users):
    """Transform user data"""
    for user in users:
        yield {
            'full_name': f"{user['first_name']} {user['last_name']}",
            'email': user['email'].lower()
        }

# Pipeline (memory efficient)
def process_users(filename):
    users = read_csv_rows(filename)
    active_users = filter_active(users)
    transformed = transform_user(active_users)
    return list(transformed)

# Each step processes one item at a time
```

---

## 🧪 Practice Questions

### 🟢 Easy

1. **Simple Calculator**: Create functions for add, subtract, multiply, divide.

2. **Greeting Generator**: Function that takes name and returns greeting based on time of day.

3. **List Sum**: Write a function to calculate sum of all elements in a list.

4. **Palindrome Checker**: Function to check if a string is palindrome.

### 🟡 Medium

5. **Prime Checker**: Create a function with memoization to check prime numbers.

6. **Decorator Practice**: Create a `@timer` decorator that prints execution time.

7. **Recursive Binary Search**: Implement binary search using recursion.

8. **Flatten Nested List**: Write a recursive function to flatten nested lists.
   ```python
   # Input: [1, [2, 3, [4, 5]], 6]
   # Output: [1, 2, 3, 4, 5, 6]
   ```

### 🔴 Hard

9. **Currying**: Implement a `curry` function that converts a multi-argument function into a chain of single-argument functions.

10. **Memoization Decorator**: Create a generic `@memoize` decorator for caching function results.

11. **Rate Limiter**: Create a decorator that limits function calls to N times per minute.

---

## 🚀 Mini Projects / Tasks

### Project 1: Text Analyzer Module

**Problem Statement:** Create a module with functions to analyze text.

**Features:**
- Word count
- Character count (with/without spaces)
- Sentence count
- Most frequent words
- Average word length
- Reading time estimate

```python
# text_analyzer.py

def word_count(text):
    """Count words in text"""
    return len(text.split())

def char_count(text, include_spaces=True):
    """Count characters"""
    if include_spaces:
        return len(text)
    return len(text.replace(" ", ""))

def sentence_count(text):
    """Count sentences"""
    import re
    return len(re.findall(r'[.!?]+', text))

def most_frequent_words(text, n=5):
    """Get n most frequent words"""
    from collections import Counter
    words = text.lower().split()
    return Counter(words).most_common(n)

def reading_time(text, wpm=200):
    """Estimate reading time in minutes"""
    words = word_count(text)
    return round(words / wpm, 1)

def analyze(text):
    """Full text analysis"""
    return {
        "words": word_count(text),
        "characters": char_count(text),
        "characters_no_spaces": char_count(text, False),
        "sentences": sentence_count(text),
        "reading_time_minutes": reading_time(text),
        "most_frequent": most_frequent_words(text)
    }
```

### Project 2: Function Composition Library

**Problem Statement:** Create a library for functional programming utilities.

**Features:**
- `compose(f, g)` - Compose two functions
- `pipe(value, *functions)` - Pass value through functions
- `curry(func)` - Curry a function
- `partial(func, *args)` - Partial application

---

## 💼 Interview Insight

### 🔥 Most Asked Questions

1. **Q: `*args` aur `**kwargs` mein kya difference hai?**
   
   **A:**
   - `*args`: Variable positional arguments (tuple)
   - `**kwargs`: Variable keyword arguments (dictionary)

2. **Q: Decorator kya hai? Ek example do.**
   
   **A:** Decorator ek function hai jo dusre function ko modify karta hai bina uska code change kiye. Login check, timing, logging ke liye use hota hai.

3. **Q: Closure kya hai?**
   
   **A:** Jab nested function apne enclosing scope ke variables ko remember karta hai, even after outer function has finished.

4. **Q: `lambda` function kab use karna chahiye?**
   
   **A:** Simple, one-liner functions ke liye, especially jab function ko argument ke taur pe pass karna ho (map, filter, sorted).

5. **Q: Generator aur normal function mein difference?**
   
   **A:**
   - Generator `yield` use karta hai, return nahi
   - Values lazy evaluate hoti hain (on-demand)
   - Memory efficient for large data

6. **Q: `global` aur `nonlocal` mein difference?**
   
   **A:**
   - `global`: Module-level variable access karne ke liye
   - `nonlocal`: Enclosing function's variable access karne ke liye

---

## ⚡ Shortcut Tricks

```python
# Trick 1: One-liner function return
add = lambda a, b: a + b

# Trick 2: Unpack arguments
def func(a, b, c):
    pass
args = [1, 2, 3]
func(*args)  # Unpack list
kwargs = {'a': 1, 'b': 2, 'c': 3}
func(**kwargs)  # Unpack dict

# Trick 3: Get function name
def my_func():
    pass
print(my_func.__name__)  # my_func

# Trick 4: Check if callable
print(callable(print))  # True
print(callable("string"))  # False

# Trick 5: Default dict with function
from collections import defaultdict
d = defaultdict(list)  # Default value is empty list
d['key'].append(1)

# Trick 6: Chain decorators
@decorator1
@decorator2
def func():
    pass
# Same as: func = decorator1(decorator2(func))

# Trick 7: Inspect function signature
import inspect
sig = inspect.signature(some_function)
print(sig.parameters)
```

---

## 📝 Summary

Is module mein humne seekha:

- ✅ Functions define aur call karna
- ✅ Parameters, arguments, aur return values
- ✅ `*args` aur `**kwargs`
- ✅ Lambda functions
- ✅ Scope aur LEGB rule
- ✅ Closures aur Decorators
- ✅ Recursion aur Generators
- ✅ Modules aur Packages
- ✅ Standard library overview

---

**Next Module:** [04_Data_Structures.md](./04_Data_Structures.md) - Lists, Tuples, Sets, Dictionaries aur Advanced Data Structures

---

<div align="center">

**Made with ❤️ for Python Learners**

[⬆️ Back to Top](#-03_functions_and_modulesmd)

</div>
