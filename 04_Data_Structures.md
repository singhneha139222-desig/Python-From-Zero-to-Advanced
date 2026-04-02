# 📘 04_Data_Structures.md

## 📌 Introduction

**Data Structures** ka matlab hai data ko organize aur store karne ka tarika. Jaise ghar mein different cheezon ke liye different storage hota hai:
- 🗄️ **Almirah** = List (ordered, can add/remove)
- 📦 **Sealed Box** = Tuple (fixed, can't change)
- 🎒 **Bag** = Set (no duplicates, unordered)
- 📒 **Address Book** = Dictionary (name → details mapping)

Sahi data structure choose karna performance ke liye crucial hai!

---

## 🧠 Core Concepts

### Data Structures Overview

| Data Structure | Ordered | Mutable | Duplicates | Syntax |
|----------------|---------|---------|------------|--------|
| **List** | ✅ Yes | ✅ Yes | ✅ Yes | `[1, 2, 3]` |
| **Tuple** | ✅ Yes | ❌ No | ✅ Yes | `(1, 2, 3)` |
| **Set** | ❌ No | ✅ Yes | ❌ No | `{1, 2, 3}` |
| **Dictionary** | ✅ Yes* | ✅ Yes | Keys: ❌ | `{"a": 1}` |
| **String** | ✅ Yes | ❌ No | ✅ Yes | `"hello"` |

*Dictionaries maintain insertion order from Python 3.7+

---

## 📊 Time Complexity Comparison

| Operation | List | Tuple | Set | Dict |
|-----------|------|-------|-----|------|
| Access by index | O(1) | O(1) | N/A | N/A |
| Access by key | N/A | N/A | N/A | O(1) |
| Search | O(n) | O(n) | O(1) | O(1) |
| Insert at end | O(1) | N/A | O(1) | O(1) |
| Insert at index | O(n) | N/A | N/A | N/A |
| Delete | O(n) | N/A | O(1) | O(1) |

---

## 💻 Code Examples

### 🟢 Basic Level

---

## 📋 LISTS

### List Basics

```python
# Creating lists
empty_list = []
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True, None]
nested = [[1, 2], [3, 4], [5, 6]]

# List from other iterables
from_string = list("hello")  # ['h', 'e', 'l', 'l', 'o']
from_range = list(range(5))  # [0, 1, 2, 3, 4]

# Accessing elements
print(numbers[0])    # 1 (first element)
print(numbers[-1])   # 5 (last element)
print(numbers[2:4])  # [3, 4] (slicing)
print(numbers[::2])  # [1, 3, 5] (every 2nd element)
print(numbers[::-1]) # [5, 4, 3, 2, 1] (reverse)

# Nested list access
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print(matrix[1][2])  # 6 (row 1, col 2)
```

### List Methods

```python
fruits = ["apple", "banana"]

# Adding elements
fruits.append("mango")           # Add at end → ['apple', 'banana', 'mango']
fruits.insert(1, "orange")       # Insert at index → ['apple', 'orange', 'banana', 'mango']
fruits.extend(["grape", "kiwi"]) # Add multiple → [..., 'grape', 'kiwi']

# Removing elements
fruits.remove("banana")          # Remove by value
popped = fruits.pop()            # Remove & return last element
popped_at = fruits.pop(0)        # Remove & return at index
fruits.clear()                   # Remove all elements

# Finding elements
numbers = [3, 1, 4, 1, 5, 9, 2, 6]
print(numbers.index(4))          # 2 (first occurrence index)
print(numbers.count(1))          # 2 (count occurrences)

# Sorting
numbers.sort()                   # Sort in place (ascending)
print(numbers)                   # [1, 1, 2, 3, 4, 5, 6, 9]
numbers.sort(reverse=True)       # Sort descending
numbers.reverse()                # Reverse in place

# sorted() returns new list (doesn't modify original)
original = [3, 1, 4]
sorted_list = sorted(original)   # original unchanged
print(original)                  # [3, 1, 4]
print(sorted_list)               # [1, 3, 4]

# Copy
copy1 = numbers.copy()           # Shallow copy
copy2 = numbers[:]               # Shallow copy using slice
copy3 = list(numbers)            # Shallow copy using constructor
```

### List Comprehensions

```python
# Basic list comprehension
squares = [x**2 for x in range(10)]
print(squares)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# With condition
even_squares = [x**2 for x in range(10) if x % 2 == 0]
print(even_squares)  # [0, 4, 16, 36, 64]

# With if-else
labels = ["even" if x % 2 == 0 else "odd" for x in range(5)]
print(labels)  # ['even', 'odd', 'even', 'odd', 'even']

# Nested comprehension
matrix = [[i*j for j in range(1, 4)] for i in range(1, 4)]
print(matrix)  # [[1, 2, 3], [2, 4, 6], [3, 6, 9]]

# Flatten nested list
nested = [[1, 2], [3, 4], [5, 6]]
flat = [num for sublist in nested for num in sublist]
print(flat)  # [1, 2, 3, 4, 5, 6]
```

---

## 📦 TUPLES

### Tuple Basics

```python
# Creating tuples
empty_tuple = ()
single = (42,)          # Note the comma! (42) is just int
numbers = (1, 2, 3, 4, 5)
mixed = (1, "hello", 3.14)

# Tuple packing and unpacking
point = 10, 20, 30      # Packing (parentheses optional)
x, y, z = point         # Unpacking
print(x, y, z)          # 10 20 30

# Extended unpacking
first, *middle, last = (1, 2, 3, 4, 5)
print(first)    # 1
print(middle)   # [2, 3, 4]
print(last)     # 5

# Swap using tuple unpacking
a, b = 5, 10
a, b = b, a
print(a, b)  # 10 5

# Accessing elements (same as list)
print(numbers[0])     # 1
print(numbers[-1])    # 5
print(numbers[1:4])   # (2, 3, 4)
```

### Tuple Methods

```python
numbers = (1, 2, 3, 2, 4, 2, 5)

# Only 2 methods for tuples
print(numbers.count(2))  # 3 (count occurrences)
print(numbers.index(3))  # 2 (first occurrence index)

# Tuple operations
t1 = (1, 2, 3)
t2 = (4, 5, 6)

# Concatenation
combined = t1 + t2      # (1, 2, 3, 4, 5, 6)

# Repetition
repeated = t1 * 3       # (1, 2, 3, 1, 2, 3, 1, 2, 3)

# Membership
print(2 in t1)          # True

# Length
print(len(t1))          # 3
```

### Why Use Tuples?

```python
# 1. Immutability = Safety
# Tuples can be dictionary keys (lists cannot)
locations = {
    (0, 0): "Origin",
    (10, 20): "Point A"
}
print(locations[(10, 20)])  # Point A

# 2. Faster than lists
import sys
list_size = sys.getsizeof([1, 2, 3, 4, 5])
tuple_size = sys.getsizeof((1, 2, 3, 4, 5))
print(f"List: {list_size} bytes, Tuple: {tuple_size} bytes")
# Tuple uses less memory

# 3. Function return values
def get_user():
    return "Rahul", 25, "Delhi"  # Returns tuple

name, age, city = get_user()

# 4. Named tuples (structured data)
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
print(p.x, p.y)     # 10 20
print(p[0], p[1])   # 10 20 (still works like tuple)
```

---

## 🎯 SETS

### Set Basics

```python
# Creating sets
empty_set = set()       # NOT {} (that's empty dict)
numbers = {1, 2, 3, 4, 5}
from_list = set([1, 2, 2, 3, 3, 3])  # {1, 2, 3} - duplicates removed

# Sets are unordered
s = {3, 1, 4, 1, 5}
print(s)  # {1, 3, 4, 5} - Order may vary, duplicates removed

# Only immutable elements allowed
valid_set = {1, 2, "hello", (1, 2)}  # OK
# invalid_set = {1, 2, [3, 4]}       # Error! List is mutable
```

### Set Methods

```python
numbers = {1, 2, 3}

# Adding elements
numbers.add(4)          # {1, 2, 3, 4}
numbers.update([5, 6])  # {1, 2, 3, 4, 5, 6}

# Removing elements
numbers.remove(6)       # Raises error if not found
numbers.discard(10)     # No error if not found
popped = numbers.pop()  # Remove and return arbitrary element
numbers.clear()         # Remove all elements

# Set operations
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

# Union (all elements)
print(a | b)            # {1, 2, 3, 4, 5, 6}
print(a.union(b))       # Same

# Intersection (common elements)
print(a & b)            # {3, 4}
print(a.intersection(b))

# Difference (in a but not in b)
print(a - b)            # {1, 2}
print(a.difference(b))

# Symmetric difference (in either but not both)
print(a ^ b)            # {1, 2, 5, 6}
print(a.symmetric_difference(b))
```

### Set Comparisons

```python
a = {1, 2, 3}
b = {1, 2, 3, 4, 5}
c = {1, 2, 3}

# Subset check
print(a <= b)           # True (a is subset of b)
print(a.issubset(b))    # True

# Superset check
print(b >= a)           # True (b is superset of a)
print(b.issuperset(a))  # True

# Equality
print(a == c)           # True

# Disjoint (no common elements)
d = {10, 20}
print(a.isdisjoint(d))  # True
```

### Frozen Sets (Immutable Sets)

```python
# Frozen set - immutable version of set
fs = frozenset([1, 2, 3, 4])

# Can be used as dictionary key
cache = {
    frozenset([1, 2]): "value1",
    frozenset([3, 4]): "value2"
}

# Can be element of another set
nested = {frozenset([1, 2]), frozenset([3, 4])}
```

---

## 📒 DICTIONARIES

### Dictionary Basics

```python
# Creating dictionaries
empty_dict = {}
person = {
    "name": "Rahul",
    "age": 25,
    "city": "Delhi"
}

# From list of tuples
from_tuples = dict([("a", 1), ("b", 2)])

# Using dict comprehension
squares = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Accessing values
print(person["name"])        # Rahul
print(person.get("age"))     # 25
print(person.get("salary"))  # None (no error)
print(person.get("salary", 0))  # 0 (default value)

# Keys must be immutable
valid = {
    "string": 1,
    42: "int key",
    (1, 2): "tuple key"
}
# invalid = {[1, 2]: "list key"}  # Error! List is mutable
```

### Dictionary Methods

```python
person = {"name": "Rahul", "age": 25}

# Adding/Updating
person["city"] = "Delhi"     # Add new key
person["age"] = 26           # Update existing
person.update({"job": "Developer", "age": 27})  # Update multiple

# Removing
del person["age"]            # Remove key
popped = person.pop("city")  # Remove and return value
popped = person.pop("x", None)  # Default if not found
last = person.popitem()      # Remove and return last item
person.clear()               # Remove all

# Getting views
person = {"name": "Rahul", "age": 25, "city": "Delhi"}
print(person.keys())         # dict_keys(['name', 'age', 'city'])
print(person.values())       # dict_values(['Rahul', 25, 'Delhi'])
print(person.items())        # dict_items([('name', 'Rahul'), ...])

# Iteration
for key in person:
    print(key, person[key])

for key, value in person.items():
    print(f"{key}: {value}")

# setdefault - Get or set default
count = {}
count.setdefault("a", 0)
count["a"] += 1
print(count)  # {'a': 1}
```

### 🟡 Intermediate Level

### Dictionary Comprehensions

```python
# Basic comprehension
squares = {x: x**2 for x in range(6)}
print(squares)  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# With condition
even_squares = {x: x**2 for x in range(10) if x % 2 == 0}
print(even_squares)  # {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}

# Transform existing dict
prices = {"apple": 100, "banana": 40, "mango": 80}
discounted = {item: price * 0.9 for item, price in prices.items()}
print(discounted)  # {'apple': 90.0, 'banana': 36.0, 'mango': 72.0}

# Swap keys and values
original = {"a": 1, "b": 2, "c": 3}
swapped = {v: k for k, v in original.items()}
print(swapped)  # {1: 'a', 2: 'b', 3: 'c'}

# Filter dict
scores = {"Rahul": 85, "Priya": 92, "Amit": 45, "Sneha": 78}
passed = {name: score for name, score in scores.items() if score >= 50}
print(passed)  # {'Rahul': 85, 'Priya': 92, 'Sneha': 78}
```

### Nested Dictionaries

```python
# Nested dictionary
students = {
    "Rahul": {
        "age": 25,
        "courses": ["Python", "Data Science"],
        "grades": {"Python": 90, "Data Science": 85}
    },
    "Priya": {
        "age": 23,
        "courses": ["Web Dev", "Python"],
        "grades": {"Web Dev": 88, "Python": 92}
    }
}

# Accessing nested data
print(students["Rahul"]["age"])                    # 25
print(students["Rahul"]["courses"][0])             # Python
print(students["Rahul"]["grades"]["Python"])       # 90

# Safe nested access
def get_nested(d, *keys, default=None):
    """Safely get nested dictionary value"""
    for key in keys:
        if isinstance(d, dict):
            d = d.get(key, default)
        else:
            return default
    return d

print(get_nested(students, "Rahul", "grades", "Python"))  # 90
print(get_nested(students, "Rahul", "salary", default=0)) # 0
```

### Collections Module

```python
from collections import (
    Counter, defaultdict, OrderedDict, 
    namedtuple, deque, ChainMap
)

# Counter - Count elements
text = "hello world"
counter = Counter(text)
print(counter)  # Counter({'l': 3, 'o': 2, ...})
print(counter.most_common(3))  # [('l', 3), ('o', 2), ('h', 1)]

# Word frequency
words = ["apple", "banana", "apple", "mango", "banana", "apple"]
word_count = Counter(words)
print(word_count)  # Counter({'apple': 3, 'banana': 2, 'mango': 1})

# defaultdict - Dict with default factory
# Regular dict
regular = {}
# regular["a"] += 1  # KeyError!

# defaultdict
dd = defaultdict(int)  # Default value is 0
dd["a"] += 1
dd["b"] += 1
dd["a"] += 1
print(dict(dd))  # {'a': 2, 'b': 1}

# defaultdict with list
dd_list = defaultdict(list)
dd_list["fruits"].append("apple")
dd_list["fruits"].append("banana")
dd_list["veggies"].append("carrot")
print(dict(dd_list))  # {'fruits': ['apple', 'banana'], 'veggies': ['carrot']}

# namedtuple - Tuple with named fields
Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
print(p.x, p.y)  # 10 20
print(p[0])      # 10 (index access still works)

# Create from dict
Person = namedtuple('Person', ['name', 'age', 'city'])
data = {'name': 'Rahul', 'age': 25, 'city': 'Delhi'}
person = Person(**data)
print(person.name)  # Rahul

# deque - Double-ended queue (efficient append/pop from both ends)
dq = deque([1, 2, 3])
dq.append(4)        # Add right: [1, 2, 3, 4]
dq.appendleft(0)    # Add left: [0, 1, 2, 3, 4]
dq.pop()            # Remove right: [0, 1, 2, 3]
dq.popleft()        # Remove left: [1, 2, 3]
dq.rotate(1)        # Rotate right: [3, 1, 2]
dq.rotate(-1)       # Rotate left: [1, 2, 3]

# Bounded deque (like circular buffer)
bounded = deque(maxlen=3)
bounded.extend([1, 2, 3])
bounded.append(4)   # [2, 3, 4] - oldest removed
```

### 🔴 Advanced Level

### Efficient Data Structure Operations

```python
# List vs Set for membership testing
import time

# Create large data
data_list = list(range(1000000))
data_set = set(range(1000000))

# Search in list - O(n)
start = time.time()
999999 in data_list
list_time = time.time() - start

# Search in set - O(1)
start = time.time()
999999 in data_set
set_time = time.time() - start

print(f"List search: {list_time:.6f}s")
print(f"Set search: {set_time:.6f}s")
# Set is MUCH faster for membership testing
```

### Deep Copy vs Shallow Copy

```python
import copy

# Original nested structure
original = {
    "name": "Rahul",
    "scores": [90, 85, 88],
    "info": {"city": "Delhi"}
}

# Shallow copy - nested objects share reference
shallow = original.copy()
shallow["name"] = "Priya"         # Won't affect original
shallow["scores"].append(95)       # WILL affect original!

print(original["scores"])  # [90, 85, 88, 95] - Changed!

# Deep copy - completely independent
original = {
    "name": "Rahul",
    "scores": [90, 85, 88],
    "info": {"city": "Delhi"}
}
deep = copy.deepcopy(original)
deep["scores"].append(95)

print(original["scores"])  # [90, 85, 88] - Unchanged!
print(deep["scores"])      # [90, 85, 88, 95]
```

### Custom Sorting

```python
# Sort with key function
students = [
    {"name": "Rahul", "age": 25, "score": 85},
    {"name": "Priya", "age": 22, "score": 92},
    {"name": "Amit", "age": 28, "score": 78}
]

# Sort by age
by_age = sorted(students, key=lambda x: x["age"])
print([s["name"] for s in by_age])  # ['Priya', 'Rahul', 'Amit']

# Sort by score (descending)
by_score = sorted(students, key=lambda x: x["score"], reverse=True)
print([s["name"] for s in by_score])  # ['Priya', 'Rahul', 'Amit']

# Multiple sort criteria (score desc, then name asc)
students2 = [
    {"name": "Rahul", "score": 85},
    {"name": "Priya", "score": 85},
    {"name": "Amit", "score": 92}
]
sorted_multi = sorted(students2, key=lambda x: (-x["score"], x["name"]))
print([(s["name"], s["score"]) for s in sorted_multi])
# [('Amit', 92), ('Priya', 85), ('Rahul', 85)]

# Using operator module (faster)
from operator import itemgetter

by_score = sorted(students, key=itemgetter("score"), reverse=True)

# Sort dict by value
prices = {"apple": 50, "mango": 100, "banana": 30}
sorted_by_price = dict(sorted(prices.items(), key=lambda x: x[1]))
print(sorted_by_price)  # {'banana': 30, 'apple': 50, 'mango': 100}
```

### Merging Data Structures

```python
# Merge lists
list1 = [1, 2, 3]
list2 = [4, 5, 6]

merged = list1 + list2          # [1, 2, 3, 4, 5, 6]
merged = [*list1, *list2]       # [1, 2, 3, 4, 5, 6] (unpacking)

# Merge sets
set1 = {1, 2, 3}
set2 = {3, 4, 5}
merged = set1 | set2            # {1, 2, 3, 4, 5}
merged = set1.union(set2)       # Same

# Merge dictionaries
dict1 = {"a": 1, "b": 2}
dict2 = {"b": 3, "c": 4}

merged = {**dict1, **dict2}     # {'a': 1, 'b': 3, 'c': 4} (dict2 wins)
merged = dict1 | dict2          # Python 3.9+ (same result)

# Merge without overwriting
def merge_dicts(*dicts):
    result = {}
    for d in dicts:
        for key, value in d.items():
            if key not in result:
                result[key] = value
    return result
```

### Heapq - Heap/Priority Queue

```python
import heapq

# Create heap from list
numbers = [3, 1, 4, 1, 5, 9, 2, 6]
heapq.heapify(numbers)  # Converts to min-heap in-place
print(numbers)  # [1, 1, 2, 6, 5, 9, 4, 3] (heap structure)

# Push and pop
heapq.heappush(numbers, 0)
smallest = heapq.heappop(numbers)  # 0

# Get n smallest/largest
data = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]
print(heapq.nsmallest(3, data))  # [1, 1, 2]
print(heapq.nlargest(3, data))   # [9, 6, 5]

# With key function
students = [("Rahul", 85), ("Priya", 92), ("Amit", 78)]
top_2 = heapq.nlargest(2, students, key=lambda x: x[1])
print(top_2)  # [('Priya', 92), ('Rahul', 85)]

# Priority queue implementation
class PriorityQueue:
    def __init__(self):
        self._heap = []
        self._index = 0
    
    def push(self, item, priority):
        # Lower priority number = higher priority
        heapq.heappush(self._heap, (priority, self._index, item))
        self._index += 1
    
    def pop(self):
        return heapq.heappop(self._heap)[-1]

pq = PriorityQueue()
pq.push("Task A", 3)
pq.push("Task B", 1)  # Higher priority
pq.push("Task C", 2)

print(pq.pop())  # Task B (priority 1)
print(pq.pop())  # Task C (priority 2)
```

### Bisect - Binary Search

```python
import bisect

# Maintain sorted list
sorted_list = [1, 3, 5, 7, 9]

# Find insertion point
pos = bisect.bisect(sorted_list, 4)
print(pos)  # 2 (insert after 3)

pos_left = bisect.bisect_left(sorted_list, 5)   # 2 (insert before 5)
pos_right = bisect.bisect_right(sorted_list, 5) # 3 (insert after 5)

# Insert and maintain sorted order
bisect.insort(sorted_list, 4)
print(sorted_list)  # [1, 3, 4, 5, 7, 9]

# Grade assignment using bisect
def get_grade(score):
    breakpoints = [60, 70, 80, 90]  # F, D, C, B, A
    grades = 'FDCBA'
    return grades[bisect.bisect(breakpoints, score)]

print(get_grade(55))   # F
print(get_grade(75))   # C
print(get_grade(95))   # A
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Modifying List While Iterating

```python
# Wrong
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num % 2 == 0:
        numbers.remove(num)  # Skips elements!

# Correct - Create new list
numbers = [1, 2, 3, 4, 5]
numbers = [num for num in numbers if num % 2 != 0]
# Or iterate over copy
for num in numbers[:]:
    if num % 2 == 0:
        numbers.remove(num)
```

### ❌ Mistake 2: Using Mutable Default in Dict

```python
# Wrong - All keys share same list!
d = {}.fromkeys(['a', 'b', 'c'], [])
d['a'].append(1)
print(d)  # {'a': [1], 'b': [1], 'c': [1]} - Unexpected!

# Correct
d = {key: [] for key in ['a', 'b', 'c']}
d['a'].append(1)
print(d)  # {'a': [1], 'b': [], 'c': []}
```

### ❌ Mistake 3: Empty Set Creation

```python
# Wrong
empty_set = {}  # This is a dict!
print(type(empty_set))  # <class 'dict'>

# Correct
empty_set = set()
print(type(empty_set))  # <class 'set'>
```

### ❌ Mistake 4: List Reference vs Copy

```python
# Wrong - Both point to same list
a = [1, 2, 3]
b = a
b.append(4)
print(a)  # [1, 2, 3, 4] - a also changed!

# Correct - Create copy
a = [1, 2, 3]
b = a.copy()  # or a[:] or list(a)
b.append(4)
print(a)  # [1, 2, 3] - a unchanged
```

### ❌ Mistake 5: Dict Key Error

```python
# Wrong - Raises KeyError if key missing
person = {"name": "Rahul"}
# print(person["age"])  # KeyError!

# Correct - Use .get() with default
age = person.get("age", 0)  # Returns 0 if key missing
```

---

## 💡 Pro Tips (Industry Insights)

### 🔥 Tip 1: Choose Right Data Structure

```python
# Need fast lookup? → Use dict or set
# Need order? → Use list or OrderedDict
# Need unique elements? → Use set
# Need immutability? → Use tuple or frozenset
# Need fast append/pop from both ends? → Use deque
# Need count elements? → Use Counter

# Example: Find duplicates efficiently
def find_duplicates(items):
    seen = set()
    duplicates = set()
    for item in items:
        if item in seen:
            duplicates.add(item)
        seen.add(item)
    return duplicates

nums = [1, 2, 3, 2, 4, 3, 5]
print(find_duplicates(nums))  # {2, 3}
```

### 🔥 Tip 2: Use Generators for Large Data

```python
# Bad - Loads all data in memory
def get_squares_list(n):
    return [x**2 for x in range(n)]

# Good - Generates on demand
def get_squares_gen(n):
    for x in range(n):
        yield x**2

# Or generator expression
squares = (x**2 for x in range(1000000))
```

### 🔥 Tip 3: Use Unpacking Effectively

```python
# Multiple assignment
a, b, c = 1, 2, 3

# Swap
a, b = b, a

# First and rest
first, *rest = [1, 2, 3, 4, 5]
print(first)  # 1
print(rest)   # [2, 3, 4, 5]

# First, middle, last
first, *middle, last = [1, 2, 3, 4, 5]
print(middle)  # [2, 3, 4]

# Ignore values
_, important, _ = get_data()
```

### 🔥 Tip 4: Dictionary setdefault and defaultdict

```python
# Grouping with setdefault
students = [
    ("Rahul", "A"),
    ("Priya", "B"),
    ("Amit", "A"),
    ("Sneha", "B")
]

by_grade = {}
for name, grade in students:
    by_grade.setdefault(grade, []).append(name)
print(by_grade)  # {'A': ['Rahul', 'Amit'], 'B': ['Priya', 'Sneha']}

# Better with defaultdict
from collections import defaultdict
by_grade = defaultdict(list)
for name, grade in students:
    by_grade[grade].append(name)
```

### 🔥 Tip 5: Use enumerate and zip

```python
# Instead of range(len())
items = ['a', 'b', 'c']

# Bad
for i in range(len(items)):
    print(i, items[i])

# Good
for i, item in enumerate(items):
    print(i, item)

# Parallel iteration
names = ['Rahul', 'Priya', 'Amit']
ages = [25, 23, 27]

for name, age in zip(names, ages):
    print(f"{name}: {age}")

# Create dict from two lists
name_age_dict = dict(zip(names, ages))
```

---

## 🔍 Real-world Use Cases

### Use Case 1: Inventory Management System

```python
from collections import defaultdict

class Inventory:
    def __init__(self):
        self.items = defaultdict(lambda: {"quantity": 0, "price": 0})
    
    def add_item(self, name, quantity, price):
        self.items[name]["quantity"] += quantity
        self.items[name]["price"] = price
    
    def remove_item(self, name, quantity):
        if name in self.items:
            self.items[name]["quantity"] = max(0, self.items[name]["quantity"] - quantity)
    
    def get_value(self):
        return sum(
            item["quantity"] * item["price"] 
            for item in self.items.values()
        )
    
    def low_stock(self, threshold=10):
        return {
            name: data["quantity"] 
            for name, data in self.items.items() 
            if data["quantity"] < threshold
        }

# Usage
inv = Inventory()
inv.add_item("Apple", 100, 5)
inv.add_item("Banana", 50, 3)
inv.add_item("Mango", 5, 10)

print(f"Total value: ₹{inv.get_value()}")
print(f"Low stock: {inv.low_stock()}")
```

### Use Case 2: Caching with LRU

```python
from collections import OrderedDict

class LRUCache:
    """Least Recently Used Cache"""
    
    def __init__(self, capacity):
        self.cache = OrderedDict()
        self.capacity = capacity
    
    def get(self, key):
        if key not in self.cache:
            return None
        # Move to end (most recently used)
        self.cache.move_to_end(key)
        return self.cache[key]
    
    def put(self, key, value):
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            # Remove oldest (first item)
            self.cache.popitem(last=False)

# Usage
cache = LRUCache(3)
cache.put("a", 1)
cache.put("b", 2)
cache.put("c", 3)
print(cache.get("a"))  # 1 (moves "a" to end)
cache.put("d", 4)      # Removes "b" (oldest)
print(cache.cache)     # OrderedDict([('c', 3), ('a', 1), ('d', 4)])
```

### Use Case 3: Graph Representation

```python
from collections import defaultdict, deque

class Graph:
    def __init__(self):
        self.graph = defaultdict(list)
    
    def add_edge(self, u, v):
        self.graph[u].append(v)
        self.graph[v].append(u)  # For undirected graph
    
    def bfs(self, start):
        """Breadth-First Search"""
        visited = set()
        queue = deque([start])
        result = []
        
        while queue:
            node = queue.popleft()
            if node not in visited:
                visited.add(node)
                result.append(node)
                queue.extend(
                    neighbor for neighbor in self.graph[node] 
                    if neighbor not in visited
                )
        return result
    
    def dfs(self, start, visited=None):
        """Depth-First Search"""
        if visited is None:
            visited = set()
        
        visited.add(start)
        result = [start]
        
        for neighbor in self.graph[start]:
            if neighbor not in visited:
                result.extend(self.dfs(neighbor, visited))
        
        return result

# Usage
g = Graph()
g.add_edge(0, 1)
g.add_edge(0, 2)
g.add_edge(1, 2)
g.add_edge(2, 3)

print(f"BFS from 0: {g.bfs(0)}")  # [0, 1, 2, 3]
print(f"DFS from 0: {g.dfs(0)}")  # [0, 1, 2, 3]
```

---

## 🧪 Practice Questions

### 🟢 Easy

1. **List Operations**: Write a function to find second largest element in a list.

2. **Dict Frequency**: Count frequency of each character in a string using dictionary.

3. **Set Operations**: Given two lists, find common elements using sets.

4. **Tuple Unpacking**: Swap the first and last elements of a tuple.

### 🟡 Medium

5. **Two Sum**: Given a list and target sum, find two numbers that add up to target.
   ```python
   # Input: nums = [2, 7, 11, 15], target = 9
   # Output: [0, 1] (indices of 2 and 7)
   ```

6. **Group Anagrams**: Group list of strings into anagrams.
   ```python
   # Input: ["eat", "tea", "tan", "ate", "nat", "bat"]
   # Output: [["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
   ```

7. **Nested Dict Flatten**: Flatten a nested dictionary.
   ```python
   # Input: {"a": 1, "b": {"c": 2, "d": {"e": 3}}}
   # Output: {"a": 1, "b.c": 2, "b.d.e": 3}
   ```

8. **LRU Cache**: Implement a simple LRU cache.

### 🔴 Hard

9. **Top K Frequent**: Find k most frequent elements in a list.

10. **Merge Intervals**: Merge overlapping intervals.
    ```python
    # Input: [[1,3], [2,6], [8,10], [15,18]]
    # Output: [[1,6], [8,10], [15,18]]
    ```

11. **Subarray Sum**: Find subarray with given sum (using prefix sum + dict).

---

## 🚀 Mini Projects / Tasks

### Project 1: Contact Book Application

**Problem Statement:** Create a contact book that stores and manages contacts.

**Features:**
- Add/Remove/Update contacts
- Search by name or phone
- Group contacts by category
- Export to/Import from JSON
- Find duplicates

### Project 2: Shopping Cart System

**Problem Statement:** Build a shopping cart using appropriate data structures.

**Features:**
- Add/Remove items
- Update quantities
- Apply discounts
- Calculate totals with tax
- Stock validation

---

## 💼 Interview Insight

### 🔥 Most Asked Questions

1. **Q: List aur Tuple mein kya difference hai?**
   
   **A:**
   - List: Mutable, use square brackets `[]`
   - Tuple: Immutable, use parentheses `()`
   - Tuple is faster and can be used as dict key

2. **Q: Dictionary ka internal working kya hai?**
   
   **A:** Dictionary hash table use karta hai. Key ka hash calculate hota hai, aur wo hash index determine karta hai. Average O(1) lookup time.

3. **Q: `is` vs `==` for lists?**
   
   **A:**
   - `==`: Values compare karta hai
   - `is`: Identity (same object) compare karta hai

4. **Q: Set mein list kyun add nahi kar sakte?**
   
   **A:** Set requires hashable (immutable) elements. List is mutable, so it's not hashable. Use tuple or frozenset instead.

5. **Q: How to remove duplicates from list while maintaining order?**
   
   **A:**
   ```python
   def remove_duplicates(lst):
       seen = set()
       return [x for x in lst if not (x in seen or seen.add(x))]
   ```

---

## ⚡ Shortcut Tricks

```python
# Trick 1: Create dict from two lists
keys = ['a', 'b', 'c']
values = [1, 2, 3]
d = dict(zip(keys, values))

# Trick 2: Flatten nested list
nested = [[1, 2], [3, 4], [5]]
flat = [item for sublist in nested for item in sublist]

# Trick 3: Get unique values maintaining order
from dict import fromkeys
unique = list(dict.fromkeys([1, 2, 2, 3, 1]))  # [1, 2, 3]

# Trick 4: Invert dictionary
original = {'a': 1, 'b': 2}
inverted = {v: k for k, v in original.items()}

# Trick 5: Merge lists element-wise
a = [1, 2, 3]
b = [4, 5, 6]
merged = [x + y for x, y in zip(a, b)]  # [5, 7, 9]

# Trick 6: Find most common element
from collections import Counter
most_common = Counter([1, 2, 2, 3, 2]).most_common(1)[0][0]  # 2

# Trick 7: Sort dict by value
sorted_dict = dict(sorted(d.items(), key=lambda x: x[1]))
```

---

## 📝 Summary

Is module mein humne seekha:

- ✅ Lists - Creation, methods, comprehensions
- ✅ Tuples - Immutability, unpacking, namedtuple
- ✅ Sets - Operations, frozen sets
- ✅ Dictionaries - Methods, comprehensions, nested
- ✅ Collections module - Counter, defaultdict, deque
- ✅ Time complexity comparisons
- ✅ Deep vs Shallow copy
- ✅ Custom sorting
- ✅ Heapq and Bisect modules

---

**Next Module:** [05_Object_Oriented_Programming.md](./05_Object_Oriented_Programming.md) - Classes, Objects, Inheritance, aur OOP Concepts

---

<div align="center">

**Made with ❤️ for Python Learners**

[⬆️ Back to Top](#-04_data_structuresmd)

</div>
