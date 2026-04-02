# 📘 06_File_Handling_and_Exception.md

## 📌 Introduction

**File Handling** aur **Exception Handling** Python programming ke essential skills hain!

**File Handling** = Computer mein files read/write karna
- 📄 Text files (`.txt`, `.csv`, `.json`)
- 📊 Binary files (images, videos, executables)
- 📁 Directories aur file operations

**Exception Handling** = Errors ko gracefully handle karna
- 🛡️ Program crash hone se bachana
- 📝 Meaningful error messages dena
- 🔄 Recovery options provide karna

**Real-life analogy:**
- 📖 File reading = Book padhna
- ✍️ File writing = Diary mein likhna  
- ⚠️ Exception = "Page not found" error ko handle karna

---

## 🧠 Core Concepts

### File Operations Overview

| Operation | Mode | Description |
|-----------|------|-------------|
| Read | `'r'` | Read existing file |
| Write | `'w'` | Write (creates new/overwrites) |
| Append | `'a'` | Add to end of file |
| Read+Write | `'r+'` | Read and write |
| Write+Read | `'w+'` | Write and read (overwrites) |
| Binary | `'rb'`, `'wb'` | Binary mode |

### Exception Hierarchy

```
BaseException
├── SystemExit
├── KeyboardInterrupt
├── GeneratorExit
└── Exception
    ├── StopIteration
    ├── ArithmeticError
    │   ├── ZeroDivisionError
    │   └── OverflowError
    ├── LookupError
    │   ├── IndexError
    │   └── KeyError
    ├── OSError
    │   ├── FileNotFoundError
    │   └── PermissionError
    ├── TypeError
    ├── ValueError
    └── ...
```

---

## 💻 Code Examples - File Handling

### 🟢 Basic Level

#### Opening and Closing Files

```python
# Method 1: Manual open/close (NOT recommended)
file = open("example.txt", "r")
content = file.read()
file.close()  # Don't forget to close!

# Method 2: Using context manager (RECOMMENDED)
with open("example.txt", "r") as file:
    content = file.read()
# File automatically closed when exiting 'with' block

# Why context manager is better:
# - Automatic closing even if error occurs
# - Cleaner code
# - No resource leaks
```

#### Reading Files

```python
# Create a sample file first
with open("sample.txt", "w") as f:
    f.write("Line 1: Hello World\n")
    f.write("Line 2: Python is awesome\n")
    f.write("Line 3: Learning file handling")

# Method 1: read() - Read entire file as string
with open("sample.txt", "r") as file:
    content = file.read()
    print(content)
# Output:
# Line 1: Hello World
# Line 2: Python is awesome
# Line 3: Learning file handling

# Method 2: read(n) - Read n characters
with open("sample.txt", "r") as file:
    first_10 = file.read(10)
    print(first_10)  # "Line 1: He"

# Method 3: readline() - Read one line
with open("sample.txt", "r") as file:
    line1 = file.readline()
    line2 = file.readline()
    print(line1)  # "Line 1: Hello World\n"
    print(line2)  # "Line 2: Python is awesome\n"

# Method 4: readlines() - Read all lines as list
with open("sample.txt", "r") as file:
    lines = file.readlines()
    print(lines)
# ['Line 1: Hello World\n', 'Line 2: Python is awesome\n', ...]

# Method 5: Iterate over file (MOST EFFICIENT for large files)
with open("sample.txt", "r") as file:
    for line in file:
        print(line.strip())  # strip() removes \n
```

#### Writing Files

```python
# Mode 'w' - Write (creates new or OVERWRITES existing)
with open("output.txt", "w") as file:
    file.write("First line\n")
    file.write("Second line\n")

# Mode 'a' - Append (adds to end of file)
with open("output.txt", "a") as file:
    file.write("Third line (appended)\n")

# writelines() - Write multiple lines
lines = ["Line A\n", "Line B\n", "Line C\n"]
with open("output.txt", "w") as file:
    file.writelines(lines)

# Writing with print()
with open("output.txt", "w") as file:
    print("Using print function", file=file)
    print("Another line", file=file)

# Write formatted data
data = [("Rahul", 25), ("Priya", 23), ("Amit", 27)]
with open("users.txt", "w") as file:
    for name, age in data:
        file.write(f"{name},{age}\n")
```

### 🟡 Intermediate Level

#### Working with CSV Files

```python
import csv

# Writing CSV
data = [
    ["Name", "Age", "City"],
    ["Rahul", 25, "Delhi"],
    ["Priya", 23, "Mumbai"],
    ["Amit", 27, "Pune"]
]

with open("users.csv", "w", newline="") as file:
    writer = csv.writer(file)
    writer.writerows(data)  # Write all rows at once

# Reading CSV
with open("users.csv", "r") as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)
# Output:
# ['Name', 'Age', 'City']
# ['Rahul', '25', 'Delhi']
# ...

# DictReader - Read as dictionaries
with open("users.csv", "r") as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(f"{row['Name']} is {row['Age']} years old")

# DictWriter - Write from dictionaries
users = [
    {"name": "Rahul", "age": 25, "city": "Delhi"},
    {"name": "Priya", "age": 23, "city": "Mumbai"}
]

with open("users_dict.csv", "w", newline="") as file:
    fieldnames = ["name", "age", "city"]
    writer = csv.DictWriter(file, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerows(users)
```

#### Working with JSON Files

```python
import json

# Python dict to JSON string
data = {
    "name": "Rahul",
    "age": 25,
    "skills": ["Python", "JavaScript", "SQL"],
    "is_active": True,
    "address": {
        "city": "Delhi",
        "pin": "110001"
    }
}

# Serialize to JSON string
json_string = json.dumps(data, indent=4)
print(json_string)

# Write to JSON file
with open("user.json", "w") as file:
    json.dump(data, file, indent=4)

# Read from JSON file
with open("user.json", "r") as file:
    loaded_data = json.load(file)
    print(loaded_data["name"])  # Rahul

# Parse JSON string
json_str = '{"name": "Priya", "age": 23}'
parsed = json.loads(json_str)
print(parsed["name"])  # Priya

# Custom JSON encoding
from datetime import datetime

class DateTimeEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime):
            return obj.isoformat()
        return super().default(obj)

data_with_date = {
    "event": "Meeting",
    "date": datetime.now()
}

json_output = json.dumps(data_with_date, cls=DateTimeEncoder)
print(json_output)
```

#### File and Directory Operations (os module)

```python
import os
import shutil

# Current working directory
print(os.getcwd())

# Change directory
# os.chdir("/path/to/directory")

# List files in directory
files = os.listdir(".")
print(files)

# Check if file/directory exists
print(os.path.exists("sample.txt"))      # True/False
print(os.path.isfile("sample.txt"))      # Is it a file?
print(os.path.isdir("my_folder"))        # Is it a directory?

# Get file info
if os.path.exists("sample.txt"):
    print(os.path.getsize("sample.txt"))  # Size in bytes
    print(os.path.getmtime("sample.txt")) # Modification time

# Create directory
os.makedirs("new_folder/subfolder", exist_ok=True)

# Rename file/directory
# os.rename("old_name.txt", "new_name.txt")

# Remove file
# os.remove("file_to_delete.txt")

# Remove empty directory
# os.rmdir("empty_folder")

# Remove directory with contents
# shutil.rmtree("folder_with_contents")

# Copy file
# shutil.copy("source.txt", "destination.txt")
# shutil.copy2("source.txt", "dest.txt")  # Preserves metadata

# Move file
# shutil.move("source.txt", "new_location/source.txt")

# Path operations
path = "/home/user/documents/file.txt"
print(os.path.dirname(path))   # /home/user/documents
print(os.path.basename(path))  # file.txt
print(os.path.splitext(path))  # ('/home/user/documents/file', '.txt')
print(os.path.join("folder", "subfolder", "file.txt"))
# folder/subfolder/file.txt (or folder\subfolder\file.txt on Windows)
```

#### pathlib Module (Modern Approach)

```python
from pathlib import Path

# Create path object
path = Path("my_folder/data.txt")

# Path operations
print(path.parent)      # my_folder
print(path.name)        # data.txt
print(path.stem)        # data
print(path.suffix)      # .txt
print(path.exists())    # True/False

# Get current directory
current = Path.cwd()
home = Path.home()

# Create directories
Path("new_folder/subfolder").mkdir(parents=True, exist_ok=True)

# List files with glob patterns
for py_file in Path(".").glob("*.py"):
    print(py_file)

# Recursive search
for txt_file in Path(".").rglob("*.txt"):
    print(txt_file)

# Read/Write with pathlib
path = Path("example.txt")
path.write_text("Hello, World!")
content = path.read_text()
print(content)

# Binary read/write
path.write_bytes(b"Binary data")
data = path.read_bytes()
```

### 🔴 Advanced Level

#### Binary File Operations

```python
# Writing binary data
data = bytes([0x48, 0x65, 0x6c, 0x6c, 0x6f])  # "Hello" in bytes

with open("binary_file.bin", "wb") as file:
    file.write(data)

# Reading binary data
with open("binary_file.bin", "rb") as file:
    content = file.read()
    print(content)  # b'Hello'
    print(list(content))  # [72, 101, 108, 108, 111]

# Copy binary file (like image)
with open("source_image.png", "rb") as src:
    with open("copy_image.png", "wb") as dst:
        dst.write(src.read())

# Read in chunks (memory efficient for large files)
def copy_large_file(source, destination, chunk_size=1024*1024):
    """Copy file in chunks of 1MB"""
    with open(source, "rb") as src:
        with open(destination, "wb") as dst:
            while True:
                chunk = src.read(chunk_size)
                if not chunk:
                    break
                dst.write(chunk)
```

#### Working with Pickle (Serialization)

```python
import pickle

# Complex Python objects
data = {
    "users": [
        {"name": "Rahul", "scores": [90, 85, 88]},
        {"name": "Priya", "scores": [92, 88, 95]}
    ],
    "metadata": {
        "created": "2024-01-15",
        "version": 1.0
    }
}

# Serialize (pickle) to file
with open("data.pkl", "wb") as file:
    pickle.dump(data, file)

# Deserialize (unpickle) from file
with open("data.pkl", "rb") as file:
    loaded_data = pickle.load(file)
    print(loaded_data["users"][0]["name"])  # Rahul

# Pickle a class instance
class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def __repr__(self):
        return f"User({self.name}, {self.age})"

user = User("Rahul", 25)

with open("user.pkl", "wb") as f:
    pickle.dump(user, f)

with open("user.pkl", "rb") as f:
    loaded_user = pickle.load(f)
    print(loaded_user)  # User(Rahul, 25)

# ⚠️ WARNING: Never unpickle data from untrusted sources!
```

#### File Compression

```python
import gzip
import zipfile
import tarfile

# GZIP compression
text = "Hello World! " * 1000

# Compress and write
with gzip.open("data.txt.gz", "wt") as f:
    f.write(text)

# Read compressed file
with gzip.open("data.txt.gz", "rt") as f:
    content = f.read()
    print(len(content))  # Original size

# ZIP files
# Create ZIP
with zipfile.ZipFile("archive.zip", "w", zipfile.ZIP_DEFLATED) as zf:
    zf.write("file1.txt")
    zf.write("file2.txt")
    zf.writestr("new_file.txt", "Content for new file")

# Extract ZIP
with zipfile.ZipFile("archive.zip", "r") as zf:
    zf.extractall("extracted_folder")
    
    # List contents
    print(zf.namelist())
    
    # Read specific file
    content = zf.read("new_file.txt")
    print(content.decode())

# TAR files
# Create tar.gz
with tarfile.open("archive.tar.gz", "w:gz") as tar:
    tar.add("folder_to_compress")

# Extract tar.gz
with tarfile.open("archive.tar.gz", "r:gz") as tar:
    tar.extractall("extracted")
```

#### Temporary Files

```python
import tempfile
import os

# Temporary file (automatically deleted)
with tempfile.NamedTemporaryFile(mode='w', delete=False, suffix='.txt') as f:
    f.write("Temporary data")
    temp_path = f.name
    print(f"Temp file: {temp_path}")

# Read temp file
with open(temp_path, 'r') as f:
    print(f.read())

# Clean up manually if delete=False
os.unlink(temp_path)

# Temporary directory
with tempfile.TemporaryDirectory() as tmpdir:
    # Create files in temp directory
    temp_file = os.path.join(tmpdir, "temp.txt")
    with open(temp_file, 'w') as f:
        f.write("Temp data")
    print(f"Temp dir: {tmpdir}")
# Directory automatically deleted when exiting context
```

---

## 💻 Code Examples - Exception Handling

### 🟢 Basic Level

#### Basic try-except

```python
# Without exception handling - program crashes
# result = 10 / 0  # ZeroDivisionError!

# With exception handling
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")
    result = 0

print(f"Result: {result}")  # Result: 0

# Handling multiple exceptions
try:
    # code that might fail
    number = int(input("Enter a number: "))
    result = 100 / number
except ValueError:
    print("Invalid number format!")
except ZeroDivisionError:
    print("Cannot divide by zero!")

# Multiple exceptions in one block
try:
    # code
    pass
except (ValueError, TypeError) as e:
    print(f"Error occurred: {e}")
```

#### try-except-else-finally

```python
def divide(a, b):
    try:
        result = a / b
    except ZeroDivisionError:
        print("Error: Division by zero!")
        return None
    except TypeError:
        print("Error: Invalid types!")
        return None
    else:
        # Executes only if NO exception occurred
        print("Division successful!")
        return result
    finally:
        # ALWAYS executes (cleanup code)
        print("Cleanup: Operation completed.")

print(divide(10, 2))
# Output:
# Division successful!
# Cleanup: Operation completed.
# 5.0

print(divide(10, 0))
# Output:
# Error: Division by zero!
# Cleanup: Operation completed.
# None
```

#### Getting Exception Information

```python
try:
    file = open("nonexistent.txt", "r")
except FileNotFoundError as e:
    print(f"Error type: {type(e).__name__}")
    print(f"Error message: {e}")
    print(f"Error args: {e.args}")

# Output:
# Error type: FileNotFoundError
# Error message: [Errno 2] No such file or directory: 'nonexistent.txt'
# Error args: (2, 'No such file or directory')

# Using traceback
import traceback

try:
    1 / 0
except ZeroDivisionError:
    print("Error occurred!")
    traceback.print_exc()  # Print full traceback
```

### 🟡 Intermediate Level

#### Raising Exceptions

```python
# Raise built-in exception
def validate_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative!")
    if age > 150:
        raise ValueError("Age seems unrealistic!")
    return age

try:
    validate_age(-5)
except ValueError as e:
    print(f"Validation error: {e}")

# Re-raising exceptions
def process_data(data):
    try:
        # Some processing
        if not data:
            raise ValueError("Empty data!")
    except ValueError:
        print("Logging error...")
        raise  # Re-raise the same exception

# Chaining exceptions
try:
    try:
        1 / 0
    except ZeroDivisionError as e:
        raise RuntimeError("Calculation failed") from e
except RuntimeError as e:
    print(f"Error: {e}")
    print(f"Caused by: {e.__cause__}")
```

#### Custom Exceptions

```python
# Simple custom exception
class ValidationError(Exception):
    """Custom exception for validation errors"""
    pass

class InsufficientFundsError(Exception):
    """Raised when account has insufficient funds"""
    
    def __init__(self, balance, amount):
        self.balance = balance
        self.amount = amount
        self.deficit = amount - balance
        super().__init__(
            f"Insufficient funds! Balance: ₹{balance}, "
            f"Requested: ₹{amount}, Deficit: ₹{self.deficit}"
        )

class BankAccount:
    def __init__(self, balance=0):
        self.balance = balance
    
    def withdraw(self, amount):
        if amount > self.balance:
            raise InsufficientFundsError(self.balance, amount)
        self.balance -= amount
        return amount

# Using custom exception
account = BankAccount(1000)

try:
    account.withdraw(1500)
except InsufficientFundsError as e:
    print(f"Error: {e}")
    print(f"You need ₹{e.deficit} more")

# Output:
# Error: Insufficient funds! Balance: ₹1000, Requested: ₹1500, Deficit: ₹500
# You need ₹500 more
```

#### Exception Hierarchy for Custom Exceptions

```python
# Base exception for your application
class AppError(Exception):
    """Base exception for application"""
    pass

class DatabaseError(AppError):
    """Database related errors"""
    pass

class ConnectionError(DatabaseError):
    """Database connection errors"""
    pass

class QueryError(DatabaseError):
    """Database query errors"""
    pass

class ValidationError(AppError):
    """Data validation errors"""
    pass

class AuthenticationError(AppError):
    """Authentication related errors"""
    pass

# Catch specific or general
try:
    raise ConnectionError("Cannot connect to database")
except ConnectionError as e:
    print(f"Connection issue: {e}")
except DatabaseError as e:
    print(f"Database issue: {e}")
except AppError as e:
    print(f"Application error: {e}")
```

### 🔴 Advanced Level

#### Context Managers for Resource Management

```python
from contextlib import contextmanager

# Custom context manager using class
class DatabaseConnection:
    def __init__(self, host):
        self.host = host
        self.connection = None
    
    def __enter__(self):
        print(f"Connecting to {self.host}...")
        self.connection = f"Connection to {self.host}"
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"Closing connection to {self.host}...")
        self.connection = None
        # Return True to suppress exception, False to propagate
        if exc_type is not None:
            print(f"Error occurred: {exc_val}")
        return False  # Don't suppress exceptions

# Using context manager
with DatabaseConnection("localhost") as db:
    print(f"Using: {db.connection}")
    # Operations here

# Output:
# Connecting to localhost...
# Using: Connection to localhost
# Closing connection to localhost...

# Context manager using decorator
@contextmanager
def managed_file(filename, mode):
    """Context manager for file operations"""
    try:
        f = open(filename, mode)
        yield f
    except Exception as e:
        print(f"Error: {e}")
        raise
    finally:
        f.close()
        print("File closed")

with managed_file("test.txt", "w") as f:
    f.write("Hello!")
```

#### Assertions and Debugging

```python
# Assertions - for internal checks (NOT for user input validation)
def calculate_average(numbers):
    assert len(numbers) > 0, "List cannot be empty!"
    assert all(isinstance(n, (int, float)) for n in numbers), "All elements must be numbers!"
    return sum(numbers) / len(numbers)

# Assertions are disabled with python -O (optimize mode)
# Never use for security checks or user input validation!

# Debug with logging instead of print
import logging

logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

def risky_operation():
    logging.debug("Starting operation...")
    try:
        # Some code
        result = 10 / 0
    except ZeroDivisionError:
        logging.error("Division by zero occurred!", exc_info=True)
        raise

# try:
#     risky_operation()
# except:
#     pass
```

#### Exception Groups (Python 3.11+)

```python
# Python 3.11+ - Multiple exceptions at once
def process_batch(items):
    errors = []
    results = []
    
    for i, item in enumerate(items):
        try:
            if item < 0:
                raise ValueError(f"Negative value at index {i}")
            if item == 0:
                raise ZeroDivisionError(f"Zero at index {i}")
            results.append(100 / item)
        except Exception as e:
            errors.append(e)
    
    if errors:
        raise ExceptionGroup("Batch processing errors", errors)
    
    return results

# Handling exception groups
try:
    process_batch([10, -5, 0, 20, -3])
except* ValueError as e:
    print(f"Value errors: {e.exceptions}")
except* ZeroDivisionError as e:
    print(f"Zero division errors: {e.exceptions}")
```

#### Logging Best Practices

```python
import logging
import sys
from logging.handlers import RotatingFileHandler

# Create custom logger
def setup_logger(name, log_file=None, level=logging.INFO):
    """Setup a custom logger"""
    
    logger = logging.getLogger(name)
    logger.setLevel(level)
    
    # Formatter
    formatter = logging.Formatter(
        '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
    )
    
    # Console handler
    console_handler = logging.StreamHandler(sys.stdout)
    console_handler.setLevel(level)
    console_handler.setFormatter(formatter)
    logger.addHandler(console_handler)
    
    # File handler (optional)
    if log_file:
        file_handler = RotatingFileHandler(
            log_file,
            maxBytes=5*1024*1024,  # 5MB
            backupCount=3
        )
        file_handler.setLevel(level)
        file_handler.setFormatter(formatter)
        logger.addHandler(file_handler)
    
    return logger

# Usage
logger = setup_logger('my_app', 'app.log', logging.DEBUG)

def process_data(data):
    logger.info(f"Processing {len(data)} items")
    try:
        # Processing
        result = sum(data) / len(data)
        logger.debug(f"Result: {result}")
        return result
    except ZeroDivisionError:
        logger.error("Empty data list!", exc_info=True)
        raise
    except TypeError as e:
        logger.exception(f"Invalid data type: {e}")
        raise

# process_data([1, 2, 3])
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Bare except

```python
# Wrong - Catches EVERYTHING including KeyboardInterrupt
try:
    do_something()
except:
    pass  # Silent failure!

# Correct - Be specific
try:
    do_something()
except ValueError as e:
    handle_value_error(e)
except TypeError as e:
    handle_type_error(e)

# If you must catch all, use Exception
try:
    do_something()
except Exception as e:
    logger.error(f"Unexpected error: {e}")
    raise  # Re-raise after logging
```

### ❌ Mistake 2: Not Closing Files

```python
# Wrong - File might not be closed on error
file = open("data.txt", "r")
content = file.read()
# Error occurs here - file never closed!
process(content)
file.close()

# Correct - Use context manager
with open("data.txt", "r") as file:
    content = file.read()
    process(content)
# File automatically closed
```

### ❌ Mistake 3: Catching Exception Too Broadly

```python
# Wrong - Hides programming errors
def get_user(user_id):
    try:
        return users[user_id]
    except Exception:  # Catches KeyError AND bugs
        return None

# Correct - Be specific
def get_user(user_id):
    try:
        return users[user_id]
    except KeyError:
        return None
```

### ❌ Mistake 4: Ignoring Encoding

```python
# Wrong - May fail with non-ASCII characters
with open("data.txt", "r") as f:
    content = f.read()

# Correct - Specify encoding
with open("data.txt", "r", encoding="utf-8") as f:
    content = f.read()
```

### ❌ Mistake 5: Using print for Debugging in Production

```python
# Wrong
try:
    risky_operation()
except Exception as e:
    print(f"Error: {e}")  # Lost in production!

# Correct - Use logging
import logging
logger = logging.getLogger(__name__)

try:
    risky_operation()
except Exception as e:
    logger.error(f"Error occurred: {e}", exc_info=True)
```

---

## 💡 Pro Tips (Industry Insights)

### 🔥 Tip 1: EAFP vs LBYL

```python
# LBYL - Look Before You Leap (less Pythonic)
if key in my_dict:
    value = my_dict[key]
else:
    value = default

# EAFP - Easier to Ask Forgiveness than Permission (Pythonic)
try:
    value = my_dict[key]
except KeyError:
    value = default

# Best - Use dict.get()
value = my_dict.get(key, default)
```

### 🔥 Tip 2: Use pathlib for Path Operations

```python
from pathlib import Path

# Old way
import os
path = os.path.join("folder", "subfolder", "file.txt")
exists = os.path.exists(path)

# Modern way (pathlib)
path = Path("folder") / "subfolder" / "file.txt"
exists = path.exists()

# Read/write simplified
content = Path("file.txt").read_text(encoding="utf-8")
Path("output.txt").write_text("Hello", encoding="utf-8")
```

### 🔥 Tip 3: Structured Exception Handling

```python
def fetch_user_data(user_id):
    """Fetch user with proper error handling"""
    try:
        response = api_call(f"/users/{user_id}")
        response.raise_for_status()
        return response.json()
    
    except ConnectionError:
        logger.error("Network error")
        raise ServiceUnavailable("Cannot connect to server")
    
    except TimeoutError:
        logger.error("Request timed out")
        raise ServiceUnavailable("Server timeout")
    
    except HTTPError as e:
        if e.response.status_code == 404:
            return None  # User not found is OK
        logger.error(f"HTTP error: {e}")
        raise
    
    except JSONDecodeError:
        logger.error("Invalid JSON response")
        raise DataError("Corrupted response")
```

### 🔥 Tip 4: Use contextlib for Quick Context Managers

```python
from contextlib import contextmanager, suppress

# Quick context manager
@contextmanager
def timer(name):
    import time
    start = time.time()
    try:
        yield
    finally:
        print(f"{name}: {time.time() - start:.2f}s")

with timer("Data processing"):
    process_data()

# Suppress specific exceptions
from contextlib import suppress

with suppress(FileNotFoundError):
    os.remove("file_that_might_not_exist.txt")

# Equivalent to:
try:
    os.remove("file_that_might_not_exist.txt")
except FileNotFoundError:
    pass
```

### 🔥 Tip 5: Atomic File Writes

```python
import tempfile
import os

def atomic_write(filepath, content):
    """Write file atomically (all or nothing)"""
    directory = os.path.dirname(filepath)
    
    # Write to temp file first
    with tempfile.NamedTemporaryFile(
        mode='w',
        dir=directory,
        delete=False,
        encoding='utf-8'
    ) as tmp:
        tmp.write(content)
        temp_path = tmp.name
    
    # Atomic rename (replace)
    os.replace(temp_path, filepath)

# Usage - Either file is complete or unchanged
atomic_write("config.json", json.dumps(config))
```

---

## 🔍 Real-world Use Cases

### Use Case 1: Configuration File Manager

```python
import json
import os
from pathlib import Path
from typing import Any, Optional

class ConfigManager:
    """Manage application configuration files"""
    
    def __init__(self, config_path: str):
        self.config_path = Path(config_path)
        self._config = {}
        self._load()
    
    def _load(self):
        """Load configuration from file"""
        try:
            if self.config_path.exists():
                with open(self.config_path, 'r', encoding='utf-8') as f:
                    self._config = json.load(f)
        except json.JSONDecodeError as e:
            raise ConfigError(f"Invalid JSON in config file: {e}")
        except PermissionError:
            raise ConfigError(f"No permission to read {self.config_path}")
    
    def _save(self):
        """Save configuration to file"""
        try:
            self.config_path.parent.mkdir(parents=True, exist_ok=True)
            with open(self.config_path, 'w', encoding='utf-8') as f:
                json.dump(self._config, f, indent=2)
        except PermissionError:
            raise ConfigError(f"No permission to write {self.config_path}")
    
    def get(self, key: str, default: Any = None) -> Any:
        """Get configuration value"""
        keys = key.split('.')
        value = self._config
        
        for k in keys:
            if isinstance(value, dict) and k in value:
                value = value[k]
            else:
                return default
        
        return value
    
    def set(self, key: str, value: Any):
        """Set configuration value"""
        keys = key.split('.')
        config = self._config
        
        for k in keys[:-1]:
            config = config.setdefault(k, {})
        
        config[keys[-1]] = value
        self._save()

class ConfigError(Exception):
    pass

# Usage
config = ConfigManager("config/app.json")
config.set("database.host", "localhost")
config.set("database.port", 5432)
print(config.get("database.host"))  # localhost
```

### Use Case 2: Log File Analyzer

```python
import re
from collections import Counter
from datetime import datetime
from pathlib import Path
from typing import Iterator, Dict, List

class LogAnalyzer:
    """Analyze log files for patterns and errors"""
    
    # Common log pattern (Apache-style)
    LOG_PATTERN = re.compile(
        r'(?P<ip>\d+\.\d+\.\d+\.\d+) - - '
        r'\[(?P<timestamp>[^\]]+)\] '
        r'"(?P<method>\w+) (?P<path>[^\s]+) [^"]+" '
        r'(?P<status>\d+) (?P<size>\d+)'
    )
    
    def __init__(self, log_path: str):
        self.log_path = Path(log_path)
    
    def parse_lines(self) -> Iterator[Dict]:
        """Parse log file and yield structured entries"""
        try:
            with open(self.log_path, 'r', encoding='utf-8') as f:
                for line_num, line in enumerate(f, 1):
                    match = self.LOG_PATTERN.match(line.strip())
                    if match:
                        yield {
                            'line': line_num,
                            **match.groupdict(),
                            'status': int(match.group('status')),
                            'size': int(match.group('size'))
                        }
        except FileNotFoundError:
            raise LogAnalyzerError(f"Log file not found: {self.log_path}")
        except PermissionError:
            raise LogAnalyzerError(f"Cannot read log file: {self.log_path}")
    
    def get_error_requests(self) -> List[Dict]:
        """Get all requests with 4xx or 5xx status"""
        return [
            entry for entry in self.parse_lines()
            if entry['status'] >= 400
        ]
    
    def get_top_paths(self, n: int = 10) -> List[tuple]:
        """Get most accessed paths"""
        paths = Counter(entry['path'] for entry in self.parse_lines())
        return paths.most_common(n)
    
    def get_status_distribution(self) -> Dict[int, int]:
        """Get distribution of status codes"""
        statuses = Counter(entry['status'] for entry in self.parse_lines())
        return dict(sorted(statuses.items()))

class LogAnalyzerError(Exception):
    pass

# Usage
# analyzer = LogAnalyzer("access.log")
# print(analyzer.get_top_paths(5))
# print(analyzer.get_status_distribution())
```

### Use Case 3: Safe Database Operations

```python
import sqlite3
from contextlib import contextmanager
from typing import Optional, List, Dict, Any

class Database:
    """SQLite database wrapper with proper error handling"""
    
    def __init__(self, db_path: str):
        self.db_path = db_path
    
    @contextmanager
    def connection(self):
        """Context manager for database connection"""
        conn = None
        try:
            conn = sqlite3.connect(self.db_path)
            conn.row_factory = sqlite3.Row
            yield conn
            conn.commit()
        except sqlite3.Error as e:
            if conn:
                conn.rollback()
            raise DatabaseError(f"Database error: {e}")
        finally:
            if conn:
                conn.close()
    
    def execute(self, query: str, params: tuple = ()) -> List[Dict]:
        """Execute query and return results"""
        with self.connection() as conn:
            cursor = conn.execute(query, params)
            return [dict(row) for row in cursor.fetchall()]
    
    def execute_many(self, query: str, params_list: List[tuple]) -> int:
        """Execute query multiple times"""
        with self.connection() as conn:
            cursor = conn.executemany(query, params_list)
            return cursor.rowcount
    
    def get_by_id(self, table: str, id: int) -> Optional[Dict]:
        """Get single record by ID"""
        results = self.execute(
            f"SELECT * FROM {table} WHERE id = ?",
            (id,)
        )
        return results[0] if results else None
    
    def insert(self, table: str, data: Dict) -> int:
        """Insert record and return ID"""
        columns = ', '.join(data.keys())
        placeholders = ', '.join(['?' for _ in data])
        query = f"INSERT INTO {table} ({columns}) VALUES ({placeholders})"
        
        with self.connection() as conn:
            cursor = conn.execute(query, tuple(data.values()))
            return cursor.lastrowid

class DatabaseError(Exception):
    pass

# Usage
# db = Database("app.db")
# users = db.execute("SELECT * FROM users WHERE active = ?", (True,))
# user = db.get_by_id("users", 1)
# new_id = db.insert("users", {"name": "Rahul", "email": "rahul@example.com"})
```

---

## 🧪 Practice Questions

### 🟢 Easy

1. **File Word Count**: Read a file and count total words.

2. **Copy File**: Copy contents of one file to another.

3. **File Extension Filter**: List all `.txt` files in a directory.

4. **Simple Logger**: Create a function that logs messages to a file.

### 🟡 Medium

5. **CSV to JSON**: Convert CSV file to JSON file.

6. **Log Parser**: Parse a log file and extract error messages.

7. **Directory Size**: Calculate total size of all files in a directory (recursive).

8. **Custom Exception**: Create exception hierarchy for a banking application.

### 🔴 Hard

9. **File Watcher**: Monitor a directory for new files and process them.

10. **Large File Processor**: Process a very large file (GB) line by line efficiently.

11. **Retry Decorator**: Create a decorator that retries a function on specific exceptions.

---

## 🚀 Mini Projects / Tasks

### Project 1: Personal Diary Application

**Problem Statement:** Create a command-line diary application.

**Features:**
- Add new entries with date/time
- Search entries by date or keyword
- Export entries to different formats
- Encrypt sensitive entries
- Backup and restore

### Project 2: System Log Analyzer

**Problem Statement:** Build a tool to analyze system logs.

**Features:**
- Parse different log formats
- Filter by date range, severity
- Generate reports (top errors, trends)
- Alert on critical errors
- Export to CSV/JSON

---

## 💼 Interview Insight

### 🔥 Most Asked Questions

1. **Q: `try-except-else-finally` ka flow kya hai?**
   
   **A:**
   - `try`: Code that might raise exception
   - `except`: Handle specific exceptions
   - `else`: Runs only if NO exception (often forgotten!)
   - `finally`: ALWAYS runs (cleanup code)

2. **Q: `raise` vs `raise from` difference?**
   
   **A:**
   - `raise`: Re-raise or raise new exception
   - `raise X from Y`: Chain exceptions, shows cause

3. **Q: File reading ke different methods?**
   
   **A:**
   - `read()`: Entire file as string
   - `readline()`: One line
   - `readlines()`: All lines as list
   - Iteration: Memory efficient for large files

4. **Q: Context manager kya hai?**
   
   **A:** Object with `__enter__` and `__exit__` methods. Used with `with` statement for automatic resource management (like file closing).

5. **Q: Binary vs Text mode difference?**
   
   **A:**
   - Text mode (`'r'`, `'w'`): String I/O, encoding/decoding
   - Binary mode (`'rb'`, `'wb'`): Bytes I/O, no encoding

---

## ⚡ Shortcut Tricks

```python
# Trick 1: One-liner file read
content = open("file.txt").read()  # NOT recommended (no close)
content = Path("file.txt").read_text()  # Better!

# Trick 2: Quick file write
Path("file.txt").write_text("Hello!")

# Trick 3: Silent exception
from contextlib import suppress
with suppress(FileNotFoundError):
    os.remove("maybe_exists.txt")

# Trick 4: Multiple context managers
with open("in.txt") as f1, open("out.txt", "w") as f2:
    f2.write(f1.read())

# Trick 5: Check file existence
if Path("file.txt").exists():
    process_file()

# Trick 6: Get file extension
ext = Path("document.pdf").suffix  # ".pdf"

# Trick 7: Iterate with enumerate for line numbers
with open("file.txt") as f:
    for line_num, line in enumerate(f, 1):
        print(f"{line_num}: {line}")
```

---

## 📝 Summary

Is module mein humne seekha:

- ✅ File opening modes aur context managers
- ✅ Reading files (read, readline, readlines, iteration)
- ✅ Writing files (write, writelines, append)
- ✅ CSV aur JSON file handling
- ✅ os aur pathlib modules
- ✅ Binary files aur compression
- ✅ Exception handling (try-except-else-finally)
- ✅ Raising aur custom exceptions
- ✅ Logging best practices
- ✅ Context managers (custom)

---

**Next Module:** [07_Advanced_Python.md](./07_Advanced_Python.md) - Decorators, Generators, Iterators, aur Advanced Topics

---

<div align="center">

**Made with ❤️ for Python Learners**

[⬆️ Back to Top](#-06_file_handling_and_exceptionmd)

</div>
