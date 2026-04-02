# 📘 01_Python_Basics.md

## 📌 Introduction

**Welcome to Python Programming!** 🐍

Python ek bahut hi powerful aur beginner-friendly programming language hai. Agar tum programming mein bilkul naye ho, toh Python sabse best choice hai kyunki:

- **Simple syntax** - English jaisi readable language
- **Versatile** - Web development, Data Science, AI, Automation sab kuch kar sakte ho
- **Huge community** - Millions of developers worldwide
- **High demand** - Jobs mein sabse zyada demand hai

> 💡 **Fun Fact:** Python ka naam "Monty Python" comedy group ke naam pe rakha gaya tha, snake pe nahi!

---

## 🧠 Core Concepts

### 1. What is Python?

Python ek **high-level, interpreted, general-purpose programming language** hai.

| Term | Meaning |
|------|---------|
| High-level | Human-readable, machine details se door |
| Interpreted | Line by line execute hota hai |
| General-purpose | Kisi bhi type ka software bana sakte ho |

### 2. Python Installation

```bash
# Windows
# Download from python.org and install

# Verify installation
python --version
# Output: Python 3.x.x

# Or use
python3 --version
```

### 3. Python REPL (Interactive Mode)

```bash
# Terminal mein type karo
python

# Ab tum directly Python code run kar sakte ho
>>> print("Hello World")
Hello World
>>> 2 + 2
4
>>> exit()  # REPL se bahar aane ke liye
```

### 4. First Python Program

```python
# hello.py
print("Hello, World!")
print("Main Python seekh raha hoon!")
```

**Run kaise karein:**
```bash
python hello.py
```

**Output:**
```
Hello, World!
Main Python seekh raha hoon!
```

---

## 📊 Data Types Table

| Data Type | Description | Example | Mutable? |
|-----------|-------------|---------|----------|
| `int` | Integer numbers | `42`, `-10`, `0` | ❌ No |
| `float` | Decimal numbers | `3.14`, `-0.5` | ❌ No |
| `str` | Text/String | `"Hello"`, `'Python'` | ❌ No |
| `bool` | Boolean | `True`, `False` | ❌ No |
| `list` | Ordered collection | `[1, 2, 3]` | ✅ Yes |
| `tuple` | Immutable collection | `(1, 2, 3)` | ❌ No |
| `dict` | Key-value pairs | `{"name": "John"}` | ✅ Yes |
| `set` | Unique elements | `{1, 2, 3}` | ✅ Yes |
| `NoneType` | Null value | `None` | ❌ No |

---

## 💻 Code Examples

### 🟢 Basic Level

#### Variables and Assignment

```python
# Variables - Python mein type declare nahi karna padta
name = "Rahul"          # String
age = 25                # Integer
height = 5.9            # Float
is_student = True       # Boolean

# Multiple assignment
a, b, c = 1, 2, 3
x = y = z = 0           # Same value assign

# Print variables
print(name)             # Output: Rahul
print(age)              # Output: 25
print(type(name))       # Output: <class 'str'>
print(type(age))        # Output: <class 'int'>
```

#### Basic Input/Output

```python
# Input from user
name = input("Apna naam batao: ")
print("Hello,", name)

# Input with type conversion
age = int(input("Apni age batao: "))
print("Agle saal tumhari age hogi:", age + 1)

# Output formatting
price = 99.99
print(f"Price is: ₹{price}")              # f-string (Python 3.6+)
print("Price is: ₹{}".format(price))      # format method
print("Price is: ₹%s" % price)            # Old style (avoid)
```

**Output:**
```
Apna naam batao: Rahul
Hello, Rahul
Apni age batao: 25
Agle saal tumhari age hogi: 26
Price is: ₹99.99
```

#### Numbers and Arithmetic

```python
# Basic arithmetic
a = 10
b = 3

print(a + b)    # Addition: 13
print(a - b)    # Subtraction: 7
print(a * b)    # Multiplication: 30
print(a / b)    # Division: 3.3333...
print(a // b)   # Floor Division: 3
print(a % b)    # Modulus (remainder): 1
print(a ** b)   # Power: 1000

# Compound assignment
x = 5
x += 3          # x = x + 3 → 8
x -= 2          # x = x - 2 → 6
x *= 2          # x = x * 2 → 12
x //= 3         # x = x // 3 → 4
print(x)        # Output: 4
```

### 🟡 Intermediate Level

#### String Operations

```python
# String creation
s1 = "Hello"
s2 = 'World'
s3 = """This is a
multiline string"""

# String concatenation
full = s1 + " " + s2
print(full)             # Output: Hello World

# String repetition
print("Ha" * 3)         # Output: HaHaHa

# String indexing (0-based)
text = "Python"
print(text[0])          # Output: P
print(text[-1])         # Output: n (last character)
print(text[2:5])        # Output: tho (slicing)
print(text[:3])         # Output: Pyt
print(text[3:])         # Output: hon
print(text[::2])        # Output: Pto (step of 2)
print(text[::-1])       # Output: nohtyP (reverse)

# String methods
name = "  rahul sharma  "
print(name.upper())           # Output: "  RAHUL SHARMA  "
print(name.lower())           # Output: "  rahul sharma  "
print(name.strip())           # Output: "rahul sharma"
print(name.title())           # Output: "  Rahul Sharma  "
print(name.replace("a", "@")) # Output: "  r@hul sh@rm@  "
print(name.split())           # Output: ['rahul', 'sharma']
print(len(name))              # Output: 16

# Check substrings
print("rahul" in name)        # Output: True
print(name.startswith("  r")) # Output: True
print(name.count("a"))        # Output: 3
```

#### Type Conversion

```python
# Implicit conversion (automatic)
x = 5       # int
y = 2.5     # float
z = x + y   # int + float = float
print(z, type(z))   # Output: 7.5 <class 'float'>

# Explicit conversion (manual)
a = "100"
b = int(a)          # String to int
print(b + 50)       # Output: 150

c = 3.7
d = int(c)          # Float to int (truncates)
print(d)            # Output: 3

e = str(42)         # Int to string
print(e + " is the answer")   # Output: 42 is the answer

# List, tuple conversions
my_list = [1, 2, 3]
my_tuple = tuple(my_list)
my_set = set(my_list)
print(my_tuple)     # Output: (1, 2, 3)
print(my_set)       # Output: {1, 2, 3}
```

### 🔴 Advanced Level

#### Understanding Python Memory Model

```python
# Python mein sab kuch object hai
x = 10
print(id(x))        # Memory address of x
print(type(x))      # <class 'int'>

# Small integers are cached (-5 to 256)
a = 100
b = 100
print(a is b)       # Output: True (same object)
print(id(a), id(b)) # Same memory address

# Larger integers create new objects
c = 1000
d = 1000
print(c is d)       # Output: False (may vary)
print(c == d)       # Output: True (values are equal)

# String interning
s1 = "hello"
s2 = "hello"
print(s1 is s2)     # Output: True (interned)

s3 = "hello world"
s4 = "hello world"
print(s3 is s4)     # May be True or False (depends on context)
```

#### Advanced String Formatting

```python
# f-strings with expressions
name = "Rahul"
age = 25
print(f"{name} is {age} years old")
print(f"Next year: {age + 1}")
print(f"Name in caps: {name.upper()}")

# Number formatting
pi = 3.14159265359
print(f"Pi: {pi:.2f}")          # Output: Pi: 3.14
print(f"Pi: {pi:.4f}")          # Output: Pi: 3.1416

big_num = 1234567890
print(f"Big: {big_num:,}")      # Output: Big: 1,234,567,890
print(f"Big: {big_num:_}")      # Output: Big: 1_234_567_890

# Alignment
print(f"{'left':<10}|")         # Output: left      |
print(f"{'right':>10}|")        # Output:      right|
print(f"{'center':^10}|")       # Output:   center  |

# Binary, Hex, Octal
num = 255
print(f"Binary: {num:b}")       # Output: Binary: 11111111
print(f"Hex: {num:x}")          # Output: Hex: ff
print(f"Octal: {num:o}")        # Output: Octal: 377
```

#### Raw Strings and Escape Characters

```python
# Escape characters
print("Hello\nWorld")       # Newline
print("Hello\tWorld")       # Tab
print("She said \"Hi\"")    # Quotes
print("C:\\Users\\Rahul")   # Backslash

# Raw strings (escape characters ignored)
print(r"C:\Users\Rahul")    # Output: C:\Users\Rahul
print(r"Hello\nWorld")      # Output: Hello\nWorld

# Unicode characters
print("\u0048\u0065\u006c\u006c\u006f")  # Output: Hello
print("\N{SNAKE}")          # Output: 🐍 (if supported)
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Variable naming errors

```python
# Wrong
2name = "Rahul"     # Cannot start with number
my-name = "Rahul"   # Hyphens not allowed
class = "Python"    # Reserved keyword

# Correct
name2 = "Rahul"
my_name = "Rahul"
class_name = "Python"
```

### ❌ Mistake 2: Type mismatch

```python
# Wrong
age = "25"
print(age + 5)      # TypeError: can only concatenate str to str

# Correct
age = "25"
print(int(age) + 5) # Output: 30
# OR
age = 25
print(age + 5)      # Output: 30
```

### ❌ Mistake 3: Indentation errors

```python
# Wrong
if True:
print("Hello")      # IndentationError

# Correct
if True:
    print("Hello")  # 4 spaces or 1 tab
```

### ❌ Mistake 4: String immutability

```python
# Wrong
s = "hello"
s[0] = "H"          # TypeError: strings are immutable

# Correct
s = "hello"
s = "H" + s[1:]     # Creates new string
print(s)            # Output: Hello
```

### ❌ Mistake 5: `is` vs `==`

```python
# Wrong understanding
a = [1, 2, 3]
b = [1, 2, 3]
print(a is b)       # False (different objects)

# Correct
print(a == b)       # True (same values)
```

---

## 💡 Pro Tips (Industry Insights)

### 🔥 Tip 1: Use meaningful variable names

```python
# Bad
x = 86400
y = x * 365

# Good
SECONDS_PER_DAY = 86400
seconds_per_year = SECONDS_PER_DAY * 365
```

### 🔥 Tip 2: Constants naming convention

```python
# Constants in UPPER_CASE
MAX_CONNECTIONS = 100
API_KEY = "your-secret-key"
DATABASE_URL = "localhost:5432"
```

### 🔥 Tip 3: Use type hints (Python 3.5+)

```python
def greet(name: str) -> str:
    return f"Hello, {name}!"

age: int = 25
names: list[str] = ["Rahul", "Priya", "Amit"]
```

### 🔥 Tip 4: Use walrus operator (Python 3.8+)

```python
# Old way
n = len("hello")
if n > 3:
    print(f"Length is {n}")

# New way with walrus operator
if (n := len("hello")) > 3:
    print(f"Length is {n}")
```

### 🔥 Tip 5: Multiple returns with tuple unpacking

```python
def get_user():
    return "Rahul", 25, "Delhi"

name, age, city = get_user()
print(name, age, city)  # Output: Rahul 25 Delhi
```

---

## 🔍 Real-world Use Cases

### Use Case 1: Simple Calculator

```python
# Basic calculator
num1 = float(input("Enter first number: "))
operator = input("Enter operator (+, -, *, /): ")
num2 = float(input("Enter second number: "))

if operator == "+":
    result = num1 + num2
elif operator == "-":
    result = num1 - num2
elif operator == "*":
    result = num1 * num2
elif operator == "/":
    result = num1 / num2 if num2 != 0 else "Error: Division by zero"
else:
    result = "Invalid operator"

print(f"Result: {result}")
```

### Use Case 2: Temperature Converter

```python
def celsius_to_fahrenheit(celsius):
    return (celsius * 9/5) + 32

def fahrenheit_to_celsius(fahrenheit):
    return (fahrenheit - 32) * 5/9

# Usage
temp_c = 37
print(f"{temp_c}°C = {celsius_to_fahrenheit(temp_c):.1f}°F")
# Output: 37°C = 98.6°F

temp_f = 98.6
print(f"{temp_f}°F = {fahrenheit_to_celsius(temp_f):.1f}°C")
# Output: 98.6°F = 37.0°C
```

### Use Case 3: String Manipulation for Data Cleaning

```python
# Clean user input
raw_input = "   RaHuL   ShArMa   "
cleaned = raw_input.strip().lower().replace("   ", " ")
print(cleaned)  # Output: rahul sharma

# Email validation (basic)
email = "user@example.com"
is_valid = "@" in email and "." in email.split("@")[-1]
print(f"Email valid: {is_valid}")  # Output: Email valid: True
```

---

## 🧪 Practice Questions

### 🟢 Easy

1. **Variable Swap**: Swap two variables without using a third variable.
   ```python
   a = 5
   b = 10
   # Your code here
   # Expected: a = 10, b = 5
   ```

2. **Circle Area**: Write a program to calculate the area of a circle given its radius.

3. **Name Formatter**: Take user's first and last name, print full name in title case.

### 🟡 Medium

4. **Palindrome Check**: Check if a given string is a palindrome (reads same forwards and backwards).

5. **Word Counter**: Count the number of words in a given sentence.

6. **Extract Domain**: Extract the domain name from an email address.
   ```python
   email = "user@example.com"
   # Expected output: example.com
   ```

### 🔴 Hard

7. **String Compression**: Implement basic string compression.
   ```python
   # Input: "aabbbcccc"
   # Output: "a2b3c4"
   ```

8. **Valid Variable Name**: Check if a string is a valid Python variable name.

9. **Number to Words**: Convert a number (0-99) to its word representation.

---

## 🚀 Mini Projects / Tasks

### Project 1: Personal Information Card Generator

**Task:** Create a program that takes user details and generates a formatted info card.

```python
# Input:
# Name: Rahul Sharma
# Age: 25
# City: Delhi
# Profession: Developer

# Output:
# ╔════════════════════════════╗
# ║     PERSONAL INFO CARD     ║
# ╠════════════════════════════╣
# ║ Name: Rahul Sharma         ║
# ║ Age: 25                    ║
# ║ City: Delhi                ║
# ║ Profession: Developer      ║
# ╚════════════════════════════╝
```

### Project 2: Simple Bill Calculator

**Task:** Create a restaurant bill calculator that takes items and quantities, calculates total with tax.

**Features:**
- Take item names and prices
- Calculate subtotal
- Apply tax (18% GST)
- Show formatted bill

---

## 💼 Interview Insight

### 🔥 Most Asked Questions

1. **Q: Python mein `is` aur `==` mein kya difference hai?**
   
   **A:** `==` values compare karta hai, `is` memory reference (identity) compare karta hai.
   ```python
   a = [1, 2, 3]
   b = [1, 2, 3]
   print(a == b)  # True (same values)
   print(a is b)  # False (different objects)
   ```

2. **Q: Python dynamically typed kyun hai?**
   
   **A:** Python mein variable ka type runtime pe decide hota hai, compile time pe nahi. Ek hi variable ko different types ki values assign kar sakte ho.

3. **Q: Mutable aur Immutable mein kya difference hai?**
   
   **A:** 
   - **Mutable:** Modify kar sakte ho (list, dict, set)
   - **Immutable:** Modify nahi kar sakte (int, str, tuple)

4. **Q: `None` kya hai Python mein?**
   
   **A:** `None` Python ka null value hai. Iska matlab "no value" ya "nothing".

---

## ⚡ Shortcut Tricks

```python
# Trick 1: Swap variables in one line
a, b = b, a

# Trick 2: Multiple comparison
x = 5
print(1 < x < 10)  # True

# Trick 3: Ternary operator
age = 20
status = "Adult" if age >= 18 else "Minor"

# Trick 4: String multiplication
print("-" * 50)  # Prints 50 dashes

# Trick 5: Check empty
my_list = []
if not my_list:
    print("List is empty")

# Trick 6: Get last N characters
text = "Hello World"
print(text[-5:])  # "World"

# Trick 7: Reverse string
print(text[::-1])  # "dlroW olleH"
```

---

## 📝 Summary

Is module mein humne seekha:

- ✅ Python kya hai aur kyun use karna chahiye
- ✅ Variables aur Data Types
- ✅ Input/Output operations
- ✅ String operations aur methods
- ✅ Type conversion
- ✅ Memory model basics
- ✅ Common mistakes aur unse kaise bachein

---

**Next Module:** [02_Control_Flow.md](./02_Control_Flow.md) - Conditions, Loops, aur Decision Making

---

<div align="center">

**Made with ❤️ for Python Learners**

[⬆️ Back to Top](#-01_python_basicsmd)

</div>
