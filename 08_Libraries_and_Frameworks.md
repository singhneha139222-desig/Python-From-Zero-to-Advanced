# 📘 08_Libraries_and_Frameworks.md

## 📌 Introduction

Python ki sabse badi strength hai iski **rich ecosystem of libraries and frameworks**! Har kaam ke liye ready-made tools available hain:

| Domain | Popular Libraries/Frameworks |
|--------|------------------------------|
| 🌐 Web Development | Django, Flask, FastAPI |
| 📊 Data Science | NumPy, Pandas, Matplotlib |
| 🤖 Machine Learning | Scikit-learn, TensorFlow, PyTorch |
| 🕷️ Web Scraping | BeautifulSoup, Scrapy, Selenium |
| 🧪 Testing | pytest, unittest, mock |
| 🔧 Automation | Selenium, PyAutoGUI |
| 📡 API Development | FastAPI, Flask-RESTful |

> 💡 "Don't reinvent the wheel" - Python community ne har cheez ke liye tools banaye hain!

---

## 🧠 Package Management

### pip - Package Installer

```bash
# Install package
pip install requests

# Install specific version
pip install requests==2.28.0

# Install from requirements.txt
pip install -r requirements.txt

# Upgrade package
pip install --upgrade requests

# Uninstall package
pip uninstall requests

# List installed packages
pip list

# Show package info
pip show requests

# Generate requirements.txt
pip freeze > requirements.txt

# Install in development mode
pip install -e .
```

### Virtual Environments

```bash
# Create virtual environment
python -m venv myenv

# Activate (Windows)
myenv\Scripts\activate

# Activate (Linux/Mac)
source myenv/bin/activate

# Deactivate
deactivate

# Using venv with pip
pip install requests  # Installs in virtual environment only
```

---

## 💻 Essential Libraries

## 📡 REQUESTS - HTTP Library

### 🟢 Basic: Making HTTP Requests

```python
import requests

# GET request
response = requests.get("https://api.github.com/users/python")
print(response.status_code)  # 200
print(response.json())       # Response as dict

# GET with parameters
params = {"q": "python", "page": 1}
response = requests.get("https://api.github.com/search/repos", params=params)

# POST request
data = {"name": "test", "email": "test@example.com"}
response = requests.post("https://httpbin.org/post", json=data)

# Headers
headers = {
    "Authorization": "Bearer YOUR_TOKEN",
    "Content-Type": "application/json"
}
response = requests.get("https://api.example.com/data", headers=headers)

# Handling errors
response = requests.get("https://api.example.com/data")
response.raise_for_status()  # Raises HTTPError for bad responses

# Timeout
try:
    response = requests.get("https://api.example.com", timeout=5)
except requests.Timeout:
    print("Request timed out!")
```

### 🟡 Intermediate: Sessions and Authentication

```python
import requests

# Session (maintains cookies, connection pooling)
session = requests.Session()
session.headers.update({"User-Agent": "MyApp/1.0"})

# All requests in session share headers and cookies
response = session.get("https://api.example.com/login")
response = session.get("https://api.example.com/dashboard")

# Basic Authentication
from requests.auth import HTTPBasicAuth
response = requests.get(
    "https://api.example.com/user",
    auth=HTTPBasicAuth("username", "password")
)

# OAuth Token
headers = {"Authorization": "Bearer YOUR_ACCESS_TOKEN"}
response = requests.get("https://api.example.com/data", headers=headers)

# Uploading files
files = {"file": open("document.pdf", "rb")}
response = requests.post("https://api.example.com/upload", files=files)

# Download file
response = requests.get("https://example.com/large-file.zip", stream=True)
with open("file.zip", "wb") as f:
    for chunk in response.iter_content(chunk_size=8192):
        f.write(chunk)
```

---

## 📊 DATA SCIENCE LIBRARIES

### NumPy - Numerical Computing

```python
import numpy as np

# Creating arrays
arr = np.array([1, 2, 3, 4, 5])
print(arr)  # [1 2 3 4 5]

# Create special arrays
zeros = np.zeros((3, 3))       # 3x3 zeros
ones = np.ones((2, 4))         # 2x4 ones
identity = np.eye(3)           # 3x3 identity matrix
range_arr = np.arange(0, 10, 2)  # [0, 2, 4, 6, 8]
linspace = np.linspace(0, 1, 5)  # 5 points from 0 to 1

# Array operations (vectorized - very fast!)
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

print(a + b)       # [5 7 9]
print(a * b)       # [4 10 18]
print(a ** 2)      # [1 4 9]
print(np.sqrt(a))  # [1. 1.414 1.732]

# Array attributes
arr = np.array([[1, 2, 3], [4, 5, 6]])
print(arr.shape)   # (2, 3)
print(arr.dtype)   # int64
print(arr.ndim)    # 2

# Slicing
arr = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(arr[0, 1])     # 2 (row 0, col 1)
print(arr[:, 0])     # [1 4 7] (all rows, col 0)
print(arr[1:, 1:])   # [[5 6] [8 9]]

# Aggregation
print(arr.sum())     # 45
print(arr.mean())    # 5.0
print(arr.max())     # 9
print(arr.sum(axis=0))  # [12 15 18] (column sums)
print(arr.sum(axis=1))  # [6 15 24] (row sums)

# Reshaping
arr = np.arange(12)
reshaped = arr.reshape(3, 4)
print(reshaped)
# [[ 0  1  2  3]
#  [ 4  5  6  7]
#  [ 8  9 10 11]]

# Boolean indexing
arr = np.array([1, 2, 3, 4, 5, 6])
print(arr[arr > 3])    # [4 5 6]
print(arr[arr % 2 == 0])  # [2 4 6]

# Linear algebra
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

print(np.dot(A, B))       # Matrix multiplication
print(np.linalg.inv(A))   # Inverse
print(np.linalg.det(A))   # Determinant
```

### Pandas - Data Analysis

```python
import pandas as pd
import numpy as np

# Creating DataFrame
data = {
    "Name": ["Rahul", "Priya", "Amit", "Sneha"],
    "Age": [25, 23, 28, 24],
    "City": ["Delhi", "Mumbai", "Pune", "Delhi"],
    "Salary": [50000, 60000, 55000, 52000]
}
df = pd.DataFrame(data)
print(df)
#     Name  Age    City  Salary
# 0  Rahul   25   Delhi   50000
# 1  Priya   23  Mumbai   60000
# 2   Amit   28    Pune   55000
# 3  Sneha   24   Delhi   52000

# Reading from files
# df = pd.read_csv("data.csv")
# df = pd.read_excel("data.xlsx")
# df = pd.read_json("data.json")

# Basic info
print(df.head())      # First 5 rows
print(df.tail(2))     # Last 2 rows
print(df.shape)       # (4, 4)
print(df.columns)     # Column names
print(df.dtypes)      # Column data types
print(df.describe())  # Statistics
print(df.info())      # Summary

# Selecting data
print(df["Name"])           # Single column (Series)
print(df[["Name", "Age"]])  # Multiple columns (DataFrame)
print(df.loc[0])            # Row by label
print(df.iloc[0])           # Row by position
print(df.loc[0:2, "Name":"City"])  # Label-based slicing
print(df.iloc[0:2, 0:2])    # Position-based slicing

# Filtering
print(df[df["Age"] > 24])           # Filter by condition
print(df[df["City"] == "Delhi"])    # Filter by value
print(df[(df["Age"] > 23) & (df["Salary"] > 50000)])  # Multiple conditions
print(df.query("Age > 24 and City == 'Delhi'"))  # Query string

# Adding/Modifying columns
df["Bonus"] = df["Salary"] * 0.1
df["Full_Name"] = df["Name"] + " Sharma"

# Aggregation
print(df["Salary"].mean())
print(df["Age"].max())
print(df.groupby("City")["Salary"].mean())

# GroupBy
grouped = df.groupby("City").agg({
    "Salary": ["mean", "sum"],
    "Age": "mean"
})
print(grouped)

# Sorting
df_sorted = df.sort_values("Salary", ascending=False)
df_sorted = df.sort_values(["City", "Age"])

# Handling missing values
df_with_na = df.copy()
df_with_na.loc[0, "Age"] = np.nan
print(df_with_na.isnull().sum())  # Count nulls
df_clean = df_with_na.fillna(0)  # Fill with value
df_clean = df_with_na.dropna()   # Remove rows with null

# Apply function
df["Age_Category"] = df["Age"].apply(lambda x: "Young" if x < 25 else "Adult")

# Merge DataFrames
df1 = pd.DataFrame({"ID": [1, 2, 3], "Name": ["A", "B", "C"]})
df2 = pd.DataFrame({"ID": [1, 2, 4], "Score": [90, 85, 88]})
merged = pd.merge(df1, df2, on="ID", how="inner")  # inner, outer, left, right

# Save to file
# df.to_csv("output.csv", index=False)
# df.to_excel("output.xlsx", index=False)
# df.to_json("output.json", orient="records")
```

### Matplotlib - Data Visualization

```python
import matplotlib.pyplot as plt
import numpy as np

# Line plot
x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.figure(figsize=(10, 6))
plt.plot(x, y, label="sin(x)", color="blue", linewidth=2)
plt.plot(x, np.cos(x), label="cos(x)", color="red", linestyle="--")
plt.xlabel("X axis")
plt.ylabel("Y axis")
plt.title("Trigonometric Functions")
plt.legend()
plt.grid(True)
plt.savefig("plot.png", dpi=300)
plt.show()

# Bar chart
categories = ["Python", "JavaScript", "Java", "C++"]
values = [70, 60, 50, 40]

plt.figure(figsize=(8, 5))
plt.bar(categories, values, color=["blue", "orange", "green", "red"])
plt.xlabel("Language")
plt.ylabel("Popularity")
plt.title("Programming Language Popularity")
plt.show()

# Histogram
data = np.random.normal(100, 15, 1000)
plt.figure(figsize=(8, 5))
plt.hist(data, bins=30, color="skyblue", edgecolor="black")
plt.xlabel("Value")
plt.ylabel("Frequency")
plt.title("Normal Distribution")
plt.show()

# Scatter plot
x = np.random.randn(100)
y = x + np.random.randn(100) * 0.5

plt.figure(figsize=(8, 6))
plt.scatter(x, y, alpha=0.6, c="purple")
plt.xlabel("X")
plt.ylabel("Y")
plt.title("Scatter Plot")
plt.show()

# Subplots
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

axes[0, 0].plot(x, y)
axes[0, 0].set_title("Line Plot")

axes[0, 1].bar(["A", "B", "C"], [1, 2, 3])
axes[0, 1].set_title("Bar Chart")

axes[1, 0].hist(np.random.randn(1000), bins=20)
axes[1, 0].set_title("Histogram")

axes[1, 1].scatter(np.random.randn(50), np.random.randn(50))
axes[1, 1].set_title("Scatter Plot")

plt.tight_layout()
plt.show()

# Pie chart
sizes = [35, 30, 20, 15]
labels = ["Python", "JavaScript", "Java", "Other"]
explode = (0.1, 0, 0, 0)  # Explode first slice

plt.figure(figsize=(8, 8))
plt.pie(sizes, explode=explode, labels=labels, autopct="%1.1f%%",
        shadow=True, startangle=90)
plt.title("Programming Language Usage")
plt.show()
```

---

## 🌐 WEB FRAMEWORKS

### Flask - Micro Framework

```python
from flask import Flask, request, jsonify, render_template

app = Flask(__name__)

# Basic route
@app.route("/")
def home():
    return "Hello, World!"

# Route with parameters
@app.route("/user/<username>")
def user_profile(username):
    return f"Profile: {username}"

# Route with query parameters
@app.route("/search")
def search():
    query = request.args.get("q", "")
    return f"Searching for: {query}"

# JSON API endpoint
@app.route("/api/users", methods=["GET"])
def get_users():
    users = [
        {"id": 1, "name": "Rahul"},
        {"id": 2, "name": "Priya"}
    ]
    return jsonify(users)

# POST endpoint
@app.route("/api/users", methods=["POST"])
def create_user():
    data = request.get_json()
    # Process data
    return jsonify({"message": "User created", "user": data}), 201

# Template rendering
@app.route("/hello/<name>")
def hello(name):
    return render_template("hello.html", name=name)

# Error handlers
@app.errorhandler(404)
def not_found(error):
    return jsonify({"error": "Not found"}), 404

if __name__ == "__main__":
    app.run(debug=True, port=5000)
```

### FastAPI - Modern API Framework

```python
from fastapi import FastAPI, HTTPException, Query, Path
from pydantic import BaseModel
from typing import Optional, List

app = FastAPI(title="My API", version="1.0.0")

# Pydantic models for validation
class User(BaseModel):
    id: Optional[int] = None
    name: str
    email: str
    age: Optional[int] = None

class UserResponse(BaseModel):
    id: int
    name: str
    email: str

# In-memory database
users_db = []

# GET all users
@app.get("/users", response_model=List[UserResponse])
async def get_users():
    return users_db

# GET user by ID
@app.get("/users/{user_id}", response_model=UserResponse)
async def get_user(user_id: int = Path(..., gt=0)):
    for user in users_db:
        if user["id"] == user_id:
            return user
    raise HTTPException(status_code=404, detail="User not found")

# POST create user
@app.post("/users", response_model=UserResponse, status_code=201)
async def create_user(user: User):
    user_dict = user.dict()
    user_dict["id"] = len(users_db) + 1
    users_db.append(user_dict)
    return user_dict

# PUT update user
@app.put("/users/{user_id}", response_model=UserResponse)
async def update_user(user_id: int, user: User):
    for i, u in enumerate(users_db):
        if u["id"] == user_id:
            user_dict = user.dict()
            user_dict["id"] = user_id
            users_db[i] = user_dict
            return user_dict
    raise HTTPException(status_code=404, detail="User not found")

# DELETE user
@app.delete("/users/{user_id}")
async def delete_user(user_id: int):
    for i, user in enumerate(users_db):
        if user["id"] == user_id:
            users_db.pop(i)
            return {"message": "User deleted"}
    raise HTTPException(status_code=404, detail="User not found")

# Query parameters
@app.get("/search")
async def search(
    q: str = Query(..., min_length=1),
    limit: int = Query(10, ge=1, le=100)
):
    return {"query": q, "limit": limit}

# Run with: uvicorn main:app --reload
```

### Django - Full-Stack Framework (Overview)

```python
# Django is a full-featured web framework
# Install: pip install django
# Create project: django-admin startproject myproject
# Create app: python manage.py startapp myapp
# Run server: python manage.py runserver

# models.py - Database models
from django.db import models

class User(models.Model):
    name = models.CharField(max_length=100)
    email = models.EmailField(unique=True)
    created_at = models.DateTimeField(auto_now_add=True)
    
    def __str__(self):
        return self.name

# views.py - Request handlers
from django.http import JsonResponse
from django.views import View
from .models import User

class UserListView(View):
    def get(self, request):
        users = list(User.objects.values())
        return JsonResponse(users, safe=False)
    
    def post(self, request):
        # Handle user creation
        pass

# urls.py - URL routing
from django.urls import path
from .views import UserListView

urlpatterns = [
    path('users/', UserListView.as_view(), name='user-list'),
]

# Django also provides:
# - Admin panel (automatic CRUD interface)
# - ORM (Object-Relational Mapping)
# - Authentication system
# - Form handling
# - Template engine
# - Middleware support
# - Security features
```

---

## 🕷️ WEB SCRAPING

### BeautifulSoup

```python
import requests
from bs4 import BeautifulSoup

# Fetch webpage
url = "https://quotes.toscrape.com/"
response = requests.get(url)
soup = BeautifulSoup(response.text, "html.parser")

# Find elements
title = soup.find("title").text
print(f"Page title: {title}")

# Find all quotes
quotes = soup.find_all("div", class_="quote")
for quote in quotes:
    text = quote.find("span", class_="text").text
    author = quote.find("small", class_="author").text
    print(f"{text} - {author}")

# Find by CSS selector
quotes = soup.select("div.quote span.text")
for quote in quotes:
    print(quote.text)

# Find links
links = soup.find_all("a")
for link in links:
    href = link.get("href")
    text = link.text
    print(f"{text}: {href}")

# Navigate the tree
first_quote = soup.find("div", class_="quote")
print(first_quote.parent.name)      # Parent element
print(first_quote.find_next_sibling())  # Next sibling

# Extract text content
page_text = soup.get_text()
print(page_text[:200])
```

### Selenium - Browser Automation

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

# Setup Chrome driver
options = webdriver.ChromeOptions()
options.add_argument("--headless")  # Run without GUI
driver = webdriver.Chrome(options=options)

try:
    # Navigate to page
    driver.get("https://www.google.com")
    
    # Find element and interact
    search_box = driver.find_element(By.NAME, "q")
    search_box.send_keys("Python programming")
    search_box.send_keys(Keys.RETURN)
    
    # Wait for results
    WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.ID, "search"))
    )
    
    # Get results
    results = driver.find_elements(By.CSS_SELECTOR, "div.g")
    for result in results[:5]:
        title = result.find_element(By.TAG_NAME, "h3").text
        print(title)
    
    # Take screenshot
    driver.save_screenshot("screenshot.png")
    
finally:
    driver.quit()

# Example: Login automation
def login_example():
    driver = webdriver.Chrome()
    driver.get("https://example.com/login")
    
    # Fill form
    driver.find_element(By.ID, "username").send_keys("myuser")
    driver.find_element(By.ID, "password").send_keys("mypassword")
    driver.find_element(By.ID, "submit").click()
    
    # Wait for redirect
    WebDriverWait(driver, 10).until(
        EC.url_contains("/dashboard")
    )
    
    driver.quit()
```

---

## 🧪 TESTING

### pytest

```python
# test_example.py
import pytest

# Basic test
def test_addition():
    assert 1 + 1 == 2

def test_string():
    s = "hello"
    assert s.upper() == "HELLO"

# Test with fixtures
@pytest.fixture
def sample_data():
    """Fixture that provides test data"""
    return {"name": "Rahul", "age": 25}

def test_with_fixture(sample_data):
    assert sample_data["name"] == "Rahul"
    assert sample_data["age"] == 25

# Parameterized tests
@pytest.mark.parametrize("input,expected", [
    (1, 1),
    (2, 4),
    (3, 9),
    (4, 16),
])
def test_square(input, expected):
    assert input ** 2 == expected

# Testing exceptions
def test_division_by_zero():
    with pytest.raises(ZeroDivisionError):
        1 / 0

# Fixtures with scope
@pytest.fixture(scope="module")
def database_connection():
    """Setup once per module"""
    conn = create_connection()
    yield conn
    conn.close()

# Markers
@pytest.mark.slow
def test_slow_operation():
    import time
    time.sleep(1)
    assert True

# Run: pytest test_example.py -v
# Run marked tests: pytest -m slow
# Skip test: @pytest.mark.skip(reason="Not implemented")
```

### unittest

```python
import unittest

class TestMathOperations(unittest.TestCase):
    
    def setUp(self):
        """Run before each test"""
        self.a = 10
        self.b = 5
    
    def tearDown(self):
        """Run after each test"""
        pass
    
    def test_addition(self):
        self.assertEqual(self.a + self.b, 15)
    
    def test_subtraction(self):
        self.assertEqual(self.a - self.b, 5)
    
    def test_multiplication(self):
        self.assertEqual(self.a * self.b, 50)
    
    def test_division(self):
        self.assertEqual(self.a / self.b, 2)
    
    def test_division_by_zero(self):
        with self.assertRaises(ZeroDivisionError):
            self.a / 0
    
    def test_in(self):
        self.assertIn("a", "abc")
    
    def test_is_instance(self):
        self.assertIsInstance(self.a, int)

if __name__ == "__main__":
    unittest.main()
```

### Mocking

```python
from unittest.mock import Mock, patch, MagicMock
import requests

# Simple mock
mock = Mock()
mock.return_value = 42
print(mock())  # 42

mock.method.return_value = "Hello"
print(mock.method())  # Hello

# Mock with side effect
mock = Mock(side_effect=ValueError("Error!"))
# mock()  # Raises ValueError

# Patch decorator
def get_data():
    response = requests.get("https://api.example.com/data")
    return response.json()

@patch("requests.get")
def test_get_data(mock_get):
    mock_get.return_value.json.return_value = {"key": "value"}
    
    result = get_data()
    
    assert result == {"key": "value"}
    mock_get.assert_called_once_with("https://api.example.com/data")

# Context manager
def test_with_context():
    with patch("requests.get") as mock_get:
        mock_get.return_value.status_code = 200
        response = requests.get("https://example.com")
        assert response.status_code == 200
```

---

## 🔧 UTILITY LIBRARIES

### Regular Expressions (re)

```python
import re

# Pattern matching
text = "My phone is 123-456-7890 and email is test@example.com"

# Find all phone numbers
pattern = r"\d{3}-\d{3}-\d{4}"
phones = re.findall(pattern, text)
print(phones)  # ['123-456-7890']

# Find all emails
pattern = r"\b[\w.-]+@[\w.-]+\.\w+\b"
emails = re.findall(pattern, text)
print(emails)  # ['test@example.com']

# Search (returns first match)
match = re.search(r"\d+", text)
if match:
    print(match.group())  # 123
    print(match.start())  # 13

# Match (matches from start only)
text2 = "123 abc"
match = re.match(r"\d+", text2)
print(match.group())  # 123

# Replace
result = re.sub(r"\d", "X", text)
print(result)  # My phone is XXX-XXX-XXXX...

# Split
result = re.split(r"[,\s]+", "apple, banana orange,mango")
print(result)  # ['apple', 'banana', 'orange', 'mango']

# Groups
text = "John Smith: 25 years old"
match = re.search(r"(\w+) (\w+): (\d+)", text)
if match:
    print(match.groups())       # ('John', 'Smith', '25')
    print(match.group(1))       # John
    print(match.group(2))       # Smith

# Named groups
pattern = r"(?P<first>\w+) (?P<last>\w+): (?P<age>\d+)"
match = re.search(pattern, text)
if match:
    print(match.group("first"))  # John
    print(match.groupdict())     # {'first': 'John', 'last': 'Smith', 'age': '25'}

# Compile pattern (for repeated use)
email_pattern = re.compile(r"\b[\w.-]+@[\w.-]+\.\w+\b")
emails = email_pattern.findall(text)
```

### Date and Time (datetime)

```python
from datetime import datetime, date, time, timedelta
import pytz  # pip install pytz

# Current date/time
now = datetime.now()
print(now)  # 2024-01-15 10:30:45.123456

today = date.today()
print(today)  # 2024-01-15

# Creating datetime
dt = datetime(2024, 1, 15, 10, 30, 0)
d = date(2024, 1, 15)
t = time(10, 30, 0)

# Formatting (strftime)
print(now.strftime("%Y-%m-%d"))           # 2024-01-15
print(now.strftime("%d/%m/%Y %H:%M:%S"))  # 15/01/2024 10:30:45
print(now.strftime("%B %d, %Y"))          # January 15, 2024

# Parsing (strptime)
date_str = "2024-01-15 10:30:00"
dt = datetime.strptime(date_str, "%Y-%m-%d %H:%M:%S")

# Timedelta (duration)
delta = timedelta(days=7, hours=3, minutes=30)
future = now + delta
past = now - timedelta(weeks=2)

# Difference between dates
dt1 = datetime(2024, 1, 1)
dt2 = datetime(2024, 1, 15)
diff = dt2 - dt1
print(diff.days)  # 14

# Timezone handling
utc = pytz.UTC
ist = pytz.timezone("Asia/Kolkata")

utc_now = datetime.now(utc)
ist_now = utc_now.astimezone(ist)
print(f"UTC: {utc_now}")
print(f"IST: {ist_now}")

# ISO format
print(now.isoformat())  # 2024-01-15T10:30:45.123456
```

### Logging

```python
import logging
import sys
from logging.handlers import RotatingFileHandler

# Basic logging
logging.basicConfig(level=logging.INFO)
logging.debug("Debug message")
logging.info("Info message")
logging.warning("Warning message")
logging.error("Error message")
logging.critical("Critical message")

# Custom logger
def setup_logger(name, level=logging.INFO):
    logger = logging.getLogger(name)
    logger.setLevel(level)
    
    # Console handler
    console = logging.StreamHandler(sys.stdout)
    console.setLevel(level)
    
    # File handler with rotation
    file_handler = RotatingFileHandler(
        "app.log",
        maxBytes=5*1024*1024,  # 5MB
        backupCount=3
    )
    
    # Formatter
    formatter = logging.Formatter(
        "%(asctime)s - %(name)s - %(levelname)s - %(message)s"
    )
    console.setFormatter(formatter)
    file_handler.setFormatter(formatter)
    
    logger.addHandler(console)
    logger.addHandler(file_handler)
    
    return logger

# Usage
logger = setup_logger("my_app")
logger.info("Application started")
logger.error("Something went wrong", exc_info=True)
```

---

## 🚀 Quick Reference Tables

### Library Cheat Sheet

| Task | Library | Command |
|------|---------|---------|
| HTTP Requests | requests | `requests.get(url)` |
| JSON | json | `json.loads(string)` |
| CSV | csv/pandas | `pd.read_csv(file)` |
| Excel | pandas/openpyxl | `pd.read_excel(file)` |
| Web Scraping | bs4 | `BeautifulSoup(html, 'html.parser')` |
| API Server | fastapi | `@app.get("/")` |
| Testing | pytest | `pytest test_file.py` |
| Date/Time | datetime | `datetime.now()` |

---

## 🧪 Practice Tasks

1. **API Consumer**: Build a script to fetch data from a public API and save to CSV.

2. **Web Scraper**: Scrape product prices from an e-commerce site.

3. **Data Analysis**: Load a CSV, clean data, and create visualizations.

4. **REST API**: Build a simple CRUD API using FastAPI.

5. **Test Suite**: Write comprehensive tests for a calculator module.

---

## 💼 Interview Insight

### 🔥 Common Questions

1. **Q: Flask vs Django kab use karein?**
   **A:** Flask: Small apps, APIs, microservices. Django: Large apps with admin, ORM, auth needed.

2. **Q: requests library mein session ka use?**
   **A:** Session maintains cookies across requests, enables connection pooling.

3. **Q: NumPy arrays vs Python lists?**
   **A:** NumPy: Faster, less memory, vectorized operations. Lists: Flexible, heterogeneous.

4. **Q: pytest fixtures kya hain?**
   **A:** Reusable setup/teardown code that provides test data or resources.

---

## 📝 Summary

- ✅ Package management with pip and venv
- ✅ HTTP requests with requests library
- ✅ Data analysis with NumPy and Pandas
- ✅ Visualization with Matplotlib
- ✅ Web frameworks (Flask, FastAPI, Django)
- ✅ Web scraping (BeautifulSoup, Selenium)
- ✅ Testing (pytest, unittest, mocking)
- ✅ Utility libraries (re, datetime, logging)

---

**Next Module:** [09_Database_and_APIs.md](./09_Database_and_APIs.md) - Databases aur API Development

---

<div align="center">

**Made with ❤️ for Python Learners**

[⬆️ Back to Top](#-08_libraries_and_frameworksmd)

</div>
