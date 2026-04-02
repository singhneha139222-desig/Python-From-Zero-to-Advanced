# 📘 02_Control_Flow.md

## 📌 Introduction

**Control Flow** ka matlab hai program ke execution ka flow control karna - matlab decide karna ki kaunsa code kab run hoga.

Socho ek traffic signal ki tarah:
- 🟢 **Green light** → Go (code execute ho)
- 🔴 **Red light** → Stop (code skip ho)
- 🟡 **Yellow light** → Decision lo (condition check karo)

Python mein 3 main control flow mechanisms hain:
1. **Conditional Statements** (if-elif-else) - Decisions
2. **Loops** (for, while) - Repetition
3. **Control Statements** (break, continue, pass) - Flow modification

---

## 🧠 Core Concepts

### 1. Conditional Statements (Decision Making)

#### if Statement

```python
# Basic if
age = 20

if age >= 18:
    print("You can vote!")
    
# Output: You can vote!
```

#### if-else Statement

```python
age = 16

if age >= 18:
    print("You can vote!")
else:
    print("You cannot vote yet!")
    
# Output: You cannot vote yet!
```

#### if-elif-else Statement

```python
marks = 75

if marks >= 90:
    grade = "A+"
elif marks >= 80:
    grade = "A"
elif marks >= 70:
    grade = "B"
elif marks >= 60:
    grade = "C"
elif marks >= 50:
    grade = "D"
else:
    grade = "F"

print(f"Your grade: {grade}")
# Output: Your grade: B
```

### 2. Comparison Operators

| Operator | Meaning | Example | Result |
|----------|---------|---------|--------|
| `==` | Equal to | `5 == 5` | `True` |
| `!=` | Not equal to | `5 != 3` | `True` |
| `>` | Greater than | `5 > 3` | `True` |
| `<` | Less than | `5 < 3` | `False` |
| `>=` | Greater or equal | `5 >= 5` | `True` |
| `<=` | Less or equal | `5 <= 3` | `False` |

### 3. Logical Operators

| Operator | Meaning | Example | Result |
|----------|---------|---------|--------|
| `and` | Both True | `True and False` | `False` |
| `or` | Either True | `True or False` | `True` |
| `not` | Opposite | `not True` | `False` |

```python
# Logical operators example
age = 25
has_license = True

# Can drive if age >= 18 AND has license
if age >= 18 and has_license:
    print("You can drive!")

# Can enter if VIP OR has ticket
is_vip = False
has_ticket = True
if is_vip or has_ticket:
    print("Welcome to the event!")

# NOT operator
is_banned = False
if not is_banned:
    print("Access granted!")
```

### 4. Membership & Identity Operators

```python
# Membership operators (in, not in)
fruits = ["apple", "banana", "mango"]
print("apple" in fruits)      # True
print("grape" not in fruits)  # True

# Identity operators (is, is not)
a = [1, 2, 3]
b = a           # Same reference
c = [1, 2, 3]   # Different object

print(a is b)       # True
print(a is c)       # False
print(a is not c)   # True
```

---

## 📊 Loops Overview Table

| Loop Type | Use Case | Syntax |
|-----------|----------|--------|
| `for` | Known iterations, iterating over sequences | `for item in sequence:` |
| `while` | Unknown iterations, condition-based | `while condition:` |
| `for` + `range()` | Number-based iterations | `for i in range(n):` |

---

## 💻 Code Examples

### 🟢 Basic Level

#### For Loop Basics

```python
# Iterating over a list
fruits = ["apple", "banana", "mango"]

for fruit in fruits:
    print(fruit)

# Output:
# apple
# banana
# mango

# Iterating over a string
for char in "Python":
    print(char, end=" ")
# Output: P y t h o n 

# Using range()
for i in range(5):
    print(i, end=" ")
# Output: 0 1 2 3 4

# range(start, stop, step)
for i in range(1, 10, 2):
    print(i, end=" ")
# Output: 1 3 5 7 9

# Reverse range
for i in range(5, 0, -1):
    print(i, end=" ")
# Output: 5 4 3 2 1
```

#### While Loop Basics

```python
# Basic while loop
count = 1

while count <= 5:
    print(count)
    count += 1

# Output:
# 1
# 2
# 3
# 4
# 5

# Infinite loop with break
while True:
    user_input = input("Enter 'quit' to exit: ")
    if user_input == "quit":
        break
    print(f"You entered: {user_input}")
```

#### Nested Conditions

```python
# Nested if-else
num = 15

if num > 0:
    if num % 2 == 0:
        print("Positive Even")
    else:
        print("Positive Odd")
else:
    print("Negative or Zero")

# Output: Positive Odd
```

### 🟡 Intermediate Level

#### Nested Loops

```python
# Multiplication table
for i in range(1, 6):
    for j in range(1, 11):
        print(f"{i} x {j} = {i*j:2}", end="  ")
    print()  # New line after each row

# Pattern printing - Right triangle
n = 5
for i in range(1, n + 1):
    print("*" * i)

# Output:
# *
# **
# ***
# ****
# *****

# Inverted triangle
for i in range(n, 0, -1):
    print("*" * i)

# Output:
# *****
# ****
# ***
# **
# *
```

#### Loop with else

```python
# For-else: else executes if loop completes normally (no break)
numbers = [1, 3, 5, 7, 9]

for num in numbers:
    if num % 2 == 0:
        print(f"Found even number: {num}")
        break
else:
    print("No even number found!")

# Output: No even number found!

# While-else
count = 0
while count < 3:
    print(count)
    count += 1
else:
    print("Loop completed normally!")

# Output:
# 0
# 1
# 2
# Loop completed normally!
```

#### Control Statements: break, continue, pass

```python
# break - Exit the loop
print("Break example:")
for i in range(10):
    if i == 5:
        break
    print(i, end=" ")
# Output: 0 1 2 3 4

print("\n")

# continue - Skip current iteration
print("Continue example:")
for i in range(10):
    if i % 2 == 0:
        continue
    print(i, end=" ")
# Output: 1 3 5 7 9

print("\n")

# pass - Do nothing (placeholder)
print("Pass example:")
for i in range(5):
    if i == 2:
        pass  # TODO: Add logic later
    print(i, end=" ")
# Output: 0 1 2 3 4
```

#### Enumerate and Zip

```python
# enumerate - Get index and value
fruits = ["apple", "banana", "mango"]

for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")

# Output:
# 0: apple
# 1: banana
# 2: mango

# Start from different index
for index, fruit in enumerate(fruits, start=1):
    print(f"{index}: {fruit}")

# Output:
# 1: apple
# 2: banana
# 3: mango

# zip - Iterate over multiple sequences
names = ["Rahul", "Priya", "Amit"]
ages = [25, 23, 27]
cities = ["Delhi", "Mumbai", "Pune"]

for name, age, city in zip(names, ages, cities):
    print(f"{name} is {age} years old from {city}")

# Output:
# Rahul is 25 years old from Delhi
# Priya is 23 years old from Mumbai
# Amit is 27 years old from Pune
```

### 🔴 Advanced Level

#### List Comprehensions with Conditions

```python
# Basic list comprehension
squares = [x**2 for x in range(10)]
print(squares)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# With condition (filter)
even_squares = [x**2 for x in range(10) if x % 2 == 0]
print(even_squares)  # [0, 4, 16, 36, 64]

# With if-else (transform)
labels = ["even" if x % 2 == 0 else "odd" for x in range(5)]
print(labels)  # ['even', 'odd', 'even', 'odd', 'even']

# Nested list comprehension
matrix = [[i*j for j in range(1, 4)] for i in range(1, 4)]
print(matrix)  # [[1, 2, 3], [2, 4, 6], [3, 6, 9]]
```

#### Dictionary and Set Comprehensions

```python
# Dictionary comprehension
squares_dict = {x: x**2 for x in range(6)}
print(squares_dict)  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# With condition
even_squares_dict = {x: x**2 for x in range(10) if x % 2 == 0}
print(even_squares_dict)  # {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}

# Set comprehension
unique_lengths = {len(word) for word in ["hello", "world", "python", "hi"]}
print(unique_lengths)  # {2, 5, 6}
```

#### Generator Expressions (Memory Efficient)

```python
# List comprehension (creates entire list in memory)
list_squares = [x**2 for x in range(1000000)]

# Generator expression (creates values on-the-fly)
gen_squares = (x**2 for x in range(1000000))

# Memory difference
import sys
print(f"List size: {sys.getsizeof(list_squares)} bytes")
print(f"Generator size: {sys.getsizeof(gen_squares)} bytes")

# Iterating over generator
gen = (x**2 for x in range(5))
for value in gen:
    print(value, end=" ")
# Output: 0 1 4 9 16
```

#### Walrus Operator in Loops (Python 3.8+)

```python
# Without walrus operator
data = input("Enter data: ")
while data != "quit":
    print(f"Processing: {data}")
    data = input("Enter data: ")

# With walrus operator (cleaner)
while (data := input("Enter data: ")) != "quit":
    print(f"Processing: {data}")
```

#### Match-Case (Python 3.10+)

```python
# Structural pattern matching (like switch-case but more powerful)
def http_status(status):
    match status:
        case 200:
            return "OK"
        case 201:
            return "Created"
        case 400:
            return "Bad Request"
        case 404:
            return "Not Found"
        case 500:
            return "Internal Server Error"
        case _:
            return "Unknown Status"

print(http_status(200))  # OK
print(http_status(404))  # Not Found
print(http_status(999))  # Unknown Status

# Pattern matching with types
def process_data(data):
    match data:
        case str():
            return f"String: {data}"
        case int() | float():
            return f"Number: {data}"
        case list():
            return f"List with {len(data)} items"
        case dict():
            return f"Dict with keys: {list(data.keys())}"
        case _:
            return "Unknown type"

print(process_data("hello"))  # String: hello
print(process_data(42))       # Number: 42
print(process_data([1,2,3]))  # List with 3 items
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Infinite Loop

```python
# Wrong - Infinite loop (forgot to update counter)
count = 0
while count < 5:
    print(count)
    # count += 1  # FORGOT THIS!

# Correct
count = 0
while count < 5:
    print(count)
    count += 1  # Don't forget to update!
```

### ❌ Mistake 2: Off-by-one Error

```python
# Wrong - Misunderstanding range
for i in range(5):
    print(i)  # Prints 0, 1, 2, 3, 4 (NOT 5!)

# If you want 1 to 5
for i in range(1, 6):  # range(1, 6) gives 1, 2, 3, 4, 5
    print(i)
```

### ❌ Mistake 3: Modifying List While Iterating

```python
# Wrong - Modifying list while iterating
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num % 2 == 0:
        numbers.remove(num)  # Dangerous!
print(numbers)  # Unexpected result: [1, 3, 5]

# Correct - Create a new list
numbers = [1, 2, 3, 4, 5]
numbers = [num for num in numbers if num % 2 != 0]
print(numbers)  # [1, 3, 5]

# Or iterate over a copy
numbers = [1, 2, 3, 4, 5]
for num in numbers[:]:  # [:] creates a copy
    if num % 2 == 0:
        numbers.remove(num)
print(numbers)  # [1, 3, 5]
```

### ❌ Mistake 4: Wrong Comparison Operator

```python
# Wrong - Assignment instead of comparison
x = 5
if x = 5:  # SyntaxError!
    print("Equal")

# Correct
if x == 5:
    print("Equal")
```

### ❌ Mistake 5: Using `and`/`or` incorrectly

```python
# Wrong - Checking multiple values
x = 5
if x == 1 or 2 or 3:  # This is ALWAYS True!
    print("Found")
# Why? Because `or 2` is always truthy

# Correct
if x == 1 or x == 2 or x == 3:
    print("Found")

# Better - Using `in`
if x in [1, 2, 3]:
    print("Found")
```

---

## 💡 Pro Tips (Industry Insights)

### 🔥 Tip 1: Use `any()` and `all()` for Multiple Conditions

```python
# Instead of multiple or conditions
# if a > 0 or b > 0 or c > 0:

values = [a, b, c]
if any(v > 0 for v in values):
    print("At least one positive")

# Instead of multiple and conditions
# if a > 0 and b > 0 and c > 0:

if all(v > 0 for v in values):
    print("All positive")
```

### 🔥 Tip 2: Avoid Deep Nesting (Guard Clauses)

```python
# Bad - Deep nesting
def process_user(user):
    if user:
        if user.is_active:
            if user.has_permission:
                return "Processing..."
            else:
                return "No permission"
        else:
            return "User inactive"
    else:
        return "No user"

# Good - Guard clauses (early returns)
def process_user(user):
    if not user:
        return "No user"
    if not user.is_active:
        return "User inactive"
    if not user.has_permission:
        return "No permission"
    return "Processing..."
```

### 🔥 Tip 3: Use itertools for Complex Iterations

```python
import itertools

# Infinite counter
counter = itertools.count(start=1, step=2)
print([next(counter) for _ in range(5)])  # [1, 3, 5, 7, 9]

# Cycle through values
colors = itertools.cycle(["red", "green", "blue"])
print([next(colors) for _ in range(7)])
# ['red', 'green', 'blue', 'red', 'green', 'blue', 'red']

# Combinations and permutations
items = ['A', 'B', 'C']
print(list(itertools.combinations(items, 2)))
# [('A', 'B'), ('A', 'C'), ('B', 'C')]

print(list(itertools.permutations(items, 2)))
# [('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'C'), ('C', 'A'), ('C', 'B')]
```

### 🔥 Tip 4: Short-circuit Evaluation

```python
# Python evaluates left to right and stops early

# This won't cause an error even if user is None
user = None
if user and user.is_active:  # user is False, so user.is_active is never evaluated
    print("Active user")

# Useful for default values
name = user_input or "Guest"  # If user_input is empty, use "Guest"

# Useful for safe access
result = data and data.get("key")  # If data is None, result is None
```

### 🔥 Tip 5: Use `_` for Unused Loop Variables

```python
# When you don't need the loop variable
for _ in range(5):
    print("Hello")

# Multiple unused values
for _, value in enumerate(items):
    print(value)

# Ignoring multiple values
first, *_, last = [1, 2, 3, 4, 5]
print(first, last)  # 1 5
```

---

## 🔍 Real-world Use Cases

### Use Case 1: Input Validation with Retry

```python
def get_valid_age():
    """Get valid age from user with retry mechanism"""
    max_attempts = 3
    
    for attempt in range(1, max_attempts + 1):
        try:
            age = int(input(f"Enter your age (Attempt {attempt}/{max_attempts}): "))
            if 0 <= age <= 120:
                return age
            else:
                print("Age must be between 0 and 120")
        except ValueError:
            print("Please enter a valid number")
    
    print("Maximum attempts exceeded!")
    return None

age = get_valid_age()
if age:
    print(f"Your age is: {age}")
```

### Use Case 2: Menu-Driven Program

```python
def show_menu():
    """Display menu and handle user choice"""
    menu = """
    ╔════════════════════════╗
    ║     MAIN MENU          ║
    ╠════════════════════════╣
    ║ 1. Add Item            ║
    ║ 2. View Items          ║
    ║ 3. Delete Item         ║
    ║ 4. Exit                ║
    ╚════════════════════════╝
    """
    
    items = []
    
    while True:
        print(menu)
        choice = input("Enter your choice: ")
        
        match choice:
            case "1":
                item = input("Enter item to add: ")
                items.append(item)
                print(f"Added: {item}")
            case "2":
                if items:
                    print("Items:", ", ".join(items))
                else:
                    print("No items yet!")
            case "3":
                if items:
                    item = input("Enter item to delete: ")
                    if item in items:
                        items.remove(item)
                        print(f"Deleted: {item}")
                    else:
                        print("Item not found!")
                else:
                    print("No items to delete!")
            case "4":
                print("Goodbye!")
                break
            case _:
                print("Invalid choice!")

# Run the menu
# show_menu()
```

### Use Case 3: Prime Number Finder

```python
def is_prime(n):
    """Check if a number is prime"""
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    
    # Only check odd numbers up to square root
    for i in range(3, int(n ** 0.5) + 1, 2):
        if n % i == 0:
            return False
    return True

def find_primes(start, end):
    """Find all primes in range"""
    primes = []
    for num in range(start, end + 1):
        if is_prime(num):
            primes.append(num)
    return primes

# Find primes between 1 and 100
primes = find_primes(1, 100)
print(f"Primes (1-100): {primes}")
print(f"Count: {len(primes)}")
```

### Use Case 4: Password Strength Checker

```python
def check_password_strength(password):
    """Check password strength and return score"""
    score = 0
    feedback = []
    
    # Length check
    if len(password) >= 8:
        score += 1
    else:
        feedback.append("Password should be at least 8 characters")
    
    if len(password) >= 12:
        score += 1
    
    # Character type checks
    has_lower = any(c.islower() for c in password)
    has_upper = any(c.isupper() for c in password)
    has_digit = any(c.isdigit() for c in password)
    has_special = any(c in "!@#$%^&*()_+-=[]{}|;:,.<>?" for c in password)
    
    if has_lower:
        score += 1
    else:
        feedback.append("Add lowercase letters")
    
    if has_upper:
        score += 1
    else:
        feedback.append("Add uppercase letters")
    
    if has_digit:
        score += 1
    else:
        feedback.append("Add numbers")
    
    if has_special:
        score += 1
    else:
        feedback.append("Add special characters")
    
    # Determine strength
    if score <= 2:
        strength = "Weak"
    elif score <= 4:
        strength = "Medium"
    else:
        strength = "Strong"
    
    return {
        "score": score,
        "max_score": 6,
        "strength": strength,
        "feedback": feedback
    }

# Test
result = check_password_strength("MyP@ss123")
print(f"Strength: {result['strength']} ({result['score']}/{result['max_score']})")
if result['feedback']:
    print("Suggestions:", ", ".join(result['feedback']))
```

---

## 🧪 Practice Questions

### 🟢 Easy

1. **FizzBuzz**: Print numbers 1-100. For multiples of 3 print "Fizz", for 5 print "Buzz", for both print "FizzBuzz".

2. **Sum of Digits**: Find sum of digits of a number (e.g., 123 → 6).

3. **Factorial**: Calculate factorial of a number using a loop.

4. **Count Vowels**: Count vowels in a given string.

### 🟡 Medium

5. **Number Guessing Game**: Random number 1-100, user guesses with hints (higher/lower).

6. **Armstrong Number**: Check if a number is Armstrong (e.g., 153 = 1³ + 5³ + 3³).

7. **Pattern Printing**: Print this pattern for n=5:
   ```
       *
      ***
     *****
    *******
   *********
   ```

8. **Fibonacci Series**: Print first n Fibonacci numbers.

### 🔴 Hard

9. **Prime Factorization**: Find all prime factors of a number.

10. **Collatz Conjecture**: For any number n, if even divide by 2, if odd multiply by 3 and add 1. Count steps to reach 1.

11. **Sudoku Row/Column Validator**: Check if a row or column in Sudoku is valid (no duplicates 1-9).

---

## 🚀 Mini Projects / Tasks

### Project 1: Number Guessing Game

**Problem Statement:** Create a game where the computer picks a random number and the user has to guess it.

**Features:**
- Random number between 1-100
- User gets hints (too high/too low)
- Count attempts
- Play again option

```python
import random

def play_game():
    """Number guessing game"""
    secret = random.randint(1, 100)
    attempts = 0
    max_attempts = 10
    
    print("🎮 Number Guessing Game!")
    print(f"Guess the number between 1 and 100")
    print(f"You have {max_attempts} attempts")
    
    while attempts < max_attempts:
        try:
            guess = int(input("\nYour guess: "))
            attempts += 1
            
            if guess == secret:
                print(f"🎉 Congratulations! You got it in {attempts} attempts!")
                return True
            elif guess < secret:
                print("📈 Too low! Try higher.")
            else:
                print("📉 Too high! Try lower.")
            
            print(f"Attempts remaining: {max_attempts - attempts}")
            
        except ValueError:
            print("Please enter a valid number!")
    
    print(f"\n😢 Game Over! The number was {secret}")
    return False

# Play the game
# play_game()
```

### Project 2: ATM Machine Simulator

**Problem Statement:** Simulate an ATM with balance check, deposit, withdraw, and PIN change.

**Features:**
- PIN verification (3 attempts)
- Check balance
- Deposit money
- Withdraw money (with balance check)
- Transaction history
- Change PIN

---

## 💼 Interview Insight

### 🔥 Most Asked Questions

1. **Q: `for` loop aur `while` loop mein kya difference hai?**
   
   **A:** 
   - `for`: Jab iterations ki number pata ho (known iterations)
   - `while`: Jab condition-based termination ho (unknown iterations)

2. **Q: `break` aur `continue` mein difference?**
   
   **A:**
   - `break`: Loop se bahar nikal jaata hai completely
   - `continue`: Current iteration skip karke next iteration pe jaata hai

3. **Q: `pass` ka use kya hai?**
   
   **A:** Placeholder hai jab koi code likhna nahi hai but syntactically kuch chahiye. Empty function/class banane mein useful.

4. **Q: List comprehension kya hai?**
   
   **A:** One-liner syntax to create lists. More Pythonic and often faster than loops.
   ```python
   # Loop way
   squares = []
   for x in range(10):
       squares.append(x**2)
   
   # List comprehension
   squares = [x**2 for x in range(10)]
   ```

5. **Q: `range()` function ke 3 forms kya hain?**
   
   **A:**
   - `range(stop)`: 0 to stop-1
   - `range(start, stop)`: start to stop-1
   - `range(start, stop, step)`: start to stop-1 with step

---

## ⚡ Shortcut Tricks

```python
# Trick 1: One-liner if-else (ternary)
result = "Pass" if marks >= 40 else "Fail"

# Trick 2: Multiple conditions with in
if grade in ['A', 'B', 'C']:
    print("Passed")

# Trick 3: Chained comparison
if 0 <= x <= 100:
    print("Valid percentage")

# Trick 4: Inline swap in loop
for i in range(n // 2):
    lst[i], lst[n-1-i] = lst[n-1-i], lst[i]

# Trick 5: Enumerate for index
for i, item in enumerate(items):
    print(f"{i}: {item}")

# Trick 6: Dictionary-based switch
operations = {
    '+': lambda a, b: a + b,
    '-': lambda a, b: a - b,
    '*': lambda a, b: a * b,
    '/': lambda a, b: a / b
}
result = operations['+'](5, 3)  # 8

# Trick 7: Loop with else for search
for item in items:
    if item == target:
        print("Found!")
        break
else:
    print("Not found!")
```

---

## 📝 Summary

Is module mein humne seekha:

- ✅ Conditional statements (if, elif, else)
- ✅ Comparison aur Logical operators
- ✅ For aur While loops
- ✅ Loop control (break, continue, pass)
- ✅ Nested loops aur conditions
- ✅ List comprehensions
- ✅ Match-case (Python 3.10+)
- ✅ itertools module basics

---

**Next Module:** [03_Functions_and_Modules.md](./03_Functions_and_Modules.md) - Functions, Modules, aur Code Organization

---

<div align="center">

**Made with ❤️ for Python Learners**

[⬆️ Back to Top](#-02_control_flowmd)

</div>
