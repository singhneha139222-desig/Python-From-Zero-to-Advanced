# 📘 05_Object_Oriented_Programming.md

## 📌 Introduction

**Object-Oriented Programming (OOP)** real-world entities ko code mein represent karne ka tarika hai. Socho jaise real duniya mein har cheez ek "object" hai:

- 🚗 **Car** = Object (properties: color, speed; actions: start, stop)
- 👤 **Person** = Object (properties: name, age; actions: walk, talk)
- 📱 **Phone** = Object (properties: brand, price; actions: call, message)

**OOP ke 4 pillars:**
1. 📦 **Encapsulation** - Data hiding (private variables)
2. 🔄 **Inheritance** - Parent se properties lena
3. 🎭 **Polymorphism** - Same interface, different behavior
4. 🎨 **Abstraction** - Complexity hide karna

---

## 🧠 Core Concepts

### Class and Object

```
Class = Blueprint/Template (Recipe)
Object = Instance (Actual Dish made from recipe)
```

```python
# Class definition
class Car:
    # Class attribute (shared by all instances)
    wheels = 4
    
    # Constructor (initializer)
    def __init__(self, brand, color):
        # Instance attributes (unique to each object)
        self.brand = brand
        self.color = color
    
    # Instance method
    def start(self):
        print(f"{self.brand} is starting...")
    
    def stop(self):
        print(f"{self.brand} is stopping...")

# Creating objects (instances)
car1 = Car("Toyota", "Red")
car2 = Car("Honda", "Blue")

# Accessing attributes
print(car1.brand)      # Toyota
print(car2.color)      # Blue
print(Car.wheels)      # 4 (class attribute)

# Calling methods
car1.start()           # Toyota is starting...
car2.stop()            # Honda is stopping...
```

---

## 📊 OOP Concepts Table

| Concept | Definition | Keyword/Syntax |
|---------|------------|----------------|
| Class | Blueprint for objects | `class ClassName:` |
| Object | Instance of a class | `obj = ClassName()` |
| Constructor | Initialize object | `__init__(self)` |
| Instance Attribute | Object-specific data | `self.attribute` |
| Class Attribute | Shared data | `ClassName.attribute` |
| Instance Method | Object behavior | `def method(self):` |
| Class Method | Class-level operation | `@classmethod` |
| Static Method | Utility function | `@staticmethod` |
| Property | Controlled attribute access | `@property` |

---

## 💻 Code Examples

### 🟢 Basic Level

#### Basic Class Structure

```python
class Person:
    """A simple Person class"""
    
    # Class attribute
    species = "Homo Sapiens"
    
    # Constructor
    def __init__(self, name, age):
        """Initialize person with name and age"""
        self.name = name      # Instance attribute
        self.age = age        # Instance attribute
    
    # Instance method
    def introduce(self):
        """Person introduces themselves"""
        return f"Hi, I'm {self.name} and I'm {self.age} years old."
    
    # Instance method
    def celebrate_birthday(self):
        """Increment age by 1"""
        self.age += 1
        print(f"Happy Birthday {self.name}! You are now {self.age}.")

# Creating objects
person1 = Person("Rahul", 25)
person2 = Person("Priya", 23)

# Using methods
print(person1.introduce())
# Output: Hi, I'm Rahul and I'm 25 years old.

person1.celebrate_birthday()
# Output: Happy Birthday Rahul! You are now 26.

# Accessing class attribute
print(Person.species)       # Homo Sapiens
print(person1.species)      # Homo Sapiens
```

#### `self` Parameter Explained

```python
class Demo:
    def method(self):
        print(f"self is: {self}")
        print(f"self type: {type(self)}")

obj = Demo()
obj.method()
# Output:
# self is: <__main__.Demo object at 0x...>
# self type: <class '__main__.Demo'>

# Behind the scenes, obj.method() is same as:
Demo.method(obj)

# self is NOT a keyword, just convention
class Example:
    def greet(this):  # Works but don't do this!
        print(f"Hello from {this}")
```

#### Constructor Variations

```python
class Rectangle:
    def __init__(self, width=1, height=1):
        """Constructor with default values"""
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

# Different ways to create objects
rect1 = Rectangle()           # Uses defaults: 1x1
rect2 = Rectangle(5, 3)       # 5x3
rect3 = Rectangle(width=10)   # 10x1 (height is default)
rect4 = Rectangle(height=7)   # 1x7 (width is default)

print(rect1.area())  # 1
print(rect2.area())  # 15
```

### 🟡 Intermediate Level

#### Class Methods and Static Methods

```python
class Employee:
    # Class attributes
    company = "TechCorp"
    employee_count = 0
    
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
        Employee.employee_count += 1
    
    # Instance method - needs self
    def give_raise(self, amount):
        """Give raise to this employee"""
        self.salary += amount
    
    # Class method - needs cls (class itself)
    @classmethod
    def change_company(cls, new_name):
        """Change company name for all employees"""
        cls.company = new_name
    
    @classmethod
    def from_string(cls, emp_string):
        """Alternative constructor from string"""
        name, salary = emp_string.split("-")
        return cls(name, int(salary))
    
    # Static method - doesn't need self or cls
    @staticmethod
    def is_workday(day):
        """Check if given day is a workday"""
        return day.weekday() < 5

# Instance method
emp1 = Employee("Rahul", 50000)
emp1.give_raise(5000)
print(emp1.salary)  # 55000

# Class method
Employee.change_company("NewTech")
print(Employee.company)  # NewTech
print(emp1.company)      # NewTech (all instances affected)

# Alternative constructor
emp2 = Employee.from_string("Priya-60000")
print(emp2.name, emp2.salary)  # Priya 60000

# Static method
from datetime import date
today = date.today()
print(Employee.is_workday(today))  # True/False
```

#### Encapsulation (Data Hiding)

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self._balance = balance     # Protected (convention)
        self.__pin = "1234"         # Private (name mangling)
    
    # Getter for balance
    @property
    def balance(self):
        """Get current balance"""
        return self._balance
    
    # Setter for balance
    @balance.setter
    def balance(self, amount):
        """Set balance with validation"""
        if amount < 0:
            raise ValueError("Balance cannot be negative!")
        self._balance = amount
    
    def deposit(self, amount):
        if amount > 0:
            self._balance += amount
            return True
        return False
    
    def withdraw(self, amount):
        if 0 < amount <= self._balance:
            self._balance -= amount
            return True
        return False
    
    def __verify_pin(self, pin):
        """Private method"""
        return pin == self.__pin

# Using the class
account = BankAccount("Rahul", 1000)

# Accessing via property
print(account.balance)      # 1000

# Setting via property
account.balance = 2000
print(account.balance)      # 2000

# account.balance = -500    # Raises ValueError

# Protected attribute (accessible but shouldn't be used directly)
print(account._balance)     # 2000 (works but not recommended)

# Private attribute (name mangled)
# print(account.__pin)      # AttributeError!
print(account._BankAccount__pin)  # 1234 (accessible via mangled name)
```

#### Properties

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius
    
    @property
    def radius(self):
        """Get radius"""
        return self._radius
    
    @radius.setter
    def radius(self, value):
        """Set radius with validation"""
        if value <= 0:
            raise ValueError("Radius must be positive!")
        self._radius = value
    
    @property
    def diameter(self):
        """Calculated property (read-only)"""
        return self._radius * 2
    
    @property
    def area(self):
        """Calculated property (read-only)"""
        import math
        return math.pi * self._radius ** 2

# Usage
circle = Circle(5)
print(circle.radius)     # 5
print(circle.diameter)   # 10
print(circle.area)       # 78.5398...

circle.radius = 10       # Uses setter
print(circle.diameter)   # 20

# circle.diameter = 30   # AttributeError: can't set (no setter)
```

#### Inheritance

```python
# Parent class (Base class / Super class)
class Animal:
    def __init__(self, name, species):
        self.name = name
        self.species = species
    
    def speak(self):
        return "Some sound"
    
    def __str__(self):
        return f"{self.name} the {self.species}"

# Child class (Derived class / Sub class)
class Dog(Animal):
    def __init__(self, name, breed):
        # Call parent constructor
        super().__init__(name, species="Dog")
        self.breed = breed
    
    # Override parent method
    def speak(self):
        return "Woof!"
    
    # New method specific to Dog
    def fetch(self):
        return f"{self.name} is fetching!"

class Cat(Animal):
    def __init__(self, name):
        super().__init__(name, species="Cat")
    
    def speak(self):
        return "Meow!"

# Using inheritance
dog = Dog("Buddy", "Golden Retriever")
cat = Cat("Whiskers")

print(dog)              # Buddy the Dog
print(dog.speak())      # Woof!
print(dog.fetch())      # Buddy is fetching!
print(dog.breed)        # Golden Retriever

print(cat)              # Whiskers the Cat
print(cat.speak())      # Meow!

# isinstance and issubclass
print(isinstance(dog, Dog))       # True
print(isinstance(dog, Animal))    # True
print(issubclass(Dog, Animal))    # True
```

### 🔴 Advanced Level

#### Multiple Inheritance

```python
class Flyable:
    def fly(self):
        return "Flying high!"
    
    def move(self):
        return "Moving by flying"

class Swimmable:
    def swim(self):
        return "Swimming fast!"
    
    def move(self):
        return "Moving by swimming"

class Walkable:
    def walk(self):
        return "Walking steady!"
    
    def move(self):
        return "Moving by walking"

# Multiple inheritance
class Duck(Flyable, Swimmable, Walkable):
    def __init__(self, name):
        self.name = name
    
    # Override to use specific parent method
    def move(self):
        return Flyable.move(self)  # Choose which parent's method

# MRO - Method Resolution Order
print(Duck.__mro__)
# (<class 'Duck'>, <class 'Flyable'>, <class 'Swimmable'>, 
#  <class 'Walkable'>, <class 'object'>)

duck = Duck("Donald")
print(duck.fly())     # Flying high!
print(duck.swim())    # Swimming fast!
print(duck.walk())    # Walking steady!
print(duck.move())    # Moving by flying (uses Flyable's method)
```

#### Abstract Classes and Interfaces

```python
from abc import ABC, abstractmethod

# Abstract Base Class
class Shape(ABC):
    """Abstract class - cannot be instantiated"""
    
    @abstractmethod
    def area(self):
        """Must be implemented by subclasses"""
        pass
    
    @abstractmethod
    def perimeter(self):
        """Must be implemented by subclasses"""
        pass
    
    # Concrete method (can be used by subclasses)
    def describe(self):
        return f"I am a shape with area {self.area()}"

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        import math
        return math.pi * self.radius ** 2
    
    def perimeter(self):
        import math
        return 2 * math.pi * self.radius

# shape = Shape()  # TypeError: Can't instantiate abstract class

rect = Rectangle(5, 3)
print(rect.area())       # 15
print(rect.perimeter())  # 16
print(rect.describe())   # I am a shape with area 15

circle = Circle(5)
print(circle.area())     # 78.5398...
```

#### Magic Methods (Dunder Methods)

```python
class Vector:
    """A 2D vector class with magic methods"""
    
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    # String representation
    def __repr__(self):
        """For developers (unambiguous)"""
        return f"Vector({self.x}, {self.y})"
    
    def __str__(self):
        """For users (readable)"""
        return f"({self.x}, {self.y})"
    
    # Arithmetic operators
    def __add__(self, other):
        """Vector addition: v1 + v2"""
        return Vector(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        """Vector subtraction: v1 - v2"""
        return Vector(self.x - other.x, self.y - other.y)
    
    def __mul__(self, scalar):
        """Scalar multiplication: v * 3"""
        return Vector(self.x * scalar, self.y * scalar)
    
    def __rmul__(self, scalar):
        """Reverse multiplication: 3 * v"""
        return self.__mul__(scalar)
    
    # Comparison operators
    def __eq__(self, other):
        """Equality: v1 == v2"""
        return self.x == other.x and self.y == other.y
    
    def __lt__(self, other):
        """Less than: v1 < v2 (by magnitude)"""
        return self.magnitude() < other.magnitude()
    
    # Length (magnitude)
    def __abs__(self):
        """Magnitude: abs(v)"""
        return self.magnitude()
    
    def magnitude(self):
        return (self.x ** 2 + self.y ** 2) ** 0.5
    
    # Make it iterable
    def __iter__(self):
        yield self.x
        yield self.y
    
    # Indexing
    def __getitem__(self, index):
        if index == 0:
            return self.x
        elif index == 1:
            return self.y
        raise IndexError("Index out of range")
    
    # Length
    def __len__(self):
        return 2
    
    # Boolean
    def __bool__(self):
        """Non-zero vector is True"""
        return self.magnitude() > 0

# Using the Vector class
v1 = Vector(3, 4)
v2 = Vector(1, 2)

print(v1)              # (3, 4)
print(repr(v1))        # Vector(3, 4)

print(v1 + v2)         # (4, 6)
print(v1 - v2)         # (2, 2)
print(v1 * 2)          # (6, 8)
print(3 * v1)          # (9, 12)

print(v1 == Vector(3, 4))  # True
print(abs(v1))         # 5.0

print(list(v1))        # [3, 4]
print(v1[0])           # 3
print(len(v1))         # 2
```

#### Composition vs Inheritance

```python
# Inheritance: "is-a" relationship
# Dog IS-A Animal

# Composition: "has-a" relationship
# Car HAS-A Engine

class Engine:
    def __init__(self, horsepower):
        self.horsepower = horsepower
        self.running = False
    
    def start(self):
        self.running = True
        return "Engine started"
    
    def stop(self):
        self.running = False
        return "Engine stopped"

class Wheels:
    def __init__(self, count=4):
        self.count = count
        self.rotating = False
    
    def rotate(self):
        self.rotating = True
        return f"{self.count} wheels rotating"

# Composition - Car HAS Engine and Wheels
class Car:
    def __init__(self, brand, horsepower):
        self.brand = brand
        self.engine = Engine(horsepower)  # Composition
        self.wheels = Wheels()            # Composition
    
    def start(self):
        return f"{self.brand}: {self.engine.start()}"
    
    def drive(self):
        if self.engine.running:
            return f"{self.brand}: {self.wheels.rotate()}"
        return "Start engine first!"

car = Car("Tesla", 450)
print(car.start())   # Tesla: Engine started
print(car.drive())   # Tesla: 4 wheels rotating
```

#### Metaclasses (Advanced)

```python
# Metaclass - Class of a class
# type is the default metaclass

# Simple example: Singleton metaclass
class SingletonMeta(type):
    """Metaclass that creates singleton classes"""
    _instances = {}
    
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class Database(metaclass=SingletonMeta):
    def __init__(self):
        print("Database initialized")
        self.connection = "Connected"

# Always returns same instance
db1 = Database()  # "Database initialized"
db2 = Database()  # Nothing printed - same instance returned

print(db1 is db2)  # True

# Auto-registration metaclass
class PluginMeta(type):
    plugins = {}
    
    def __new__(mcs, name, bases, namespace):
        cls = super().__new__(mcs, name, bases, namespace)
        if name != 'Plugin':  # Don't register base class
            mcs.plugins[name] = cls
        return cls

class Plugin(metaclass=PluginMeta):
    pass

class ImagePlugin(Plugin):
    def process(self):
        return "Processing image"

class TextPlugin(Plugin):
    def process(self):
        return "Processing text"

print(PluginMeta.plugins)
# {'ImagePlugin': <class 'ImagePlugin'>, 'TextPlugin': <class 'TextPlugin'>}
```

#### Descriptors

```python
# Descriptor - Object that defines __get__, __set__, __delete__

class Validator:
    """Descriptor that validates positive numbers"""
    
    def __init__(self, min_value=0):
        self.min_value = min_value
    
    def __set_name__(self, owner, name):
        self.name = name
    
    def __get__(self, obj, type=None):
        if obj is None:
            return self
        return obj.__dict__.get(self.name, 0)
    
    def __set__(self, obj, value):
        if value < self.min_value:
            raise ValueError(
                f"{self.name} must be at least {self.min_value}"
            )
        obj.__dict__[self.name] = value

class Product:
    price = Validator(min_value=0)
    quantity = Validator(min_value=0)
    
    def __init__(self, name, price, quantity):
        self.name = name
        self.price = price        # Uses Validator.__set__
        self.quantity = quantity  # Uses Validator.__set__
    
    @property
    def total(self):
        return self.price * self.quantity

product = Product("Laptop", 50000, 5)
print(product.total)    # 250000

# product.price = -100  # ValueError: price must be at least 0
```

#### Context Managers with Classes

```python
class FileHandler:
    """Custom context manager for file handling"""
    
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):
        print(f"Opening {self.filename}")
        self.file = open(self.filename, self.mode)
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"Closing {self.filename}")
        if self.file:
            self.file.close()
        # Return True to suppress exception, False to propagate
        return False

# Using the context manager
with FileHandler("test.txt", "w") as f:
    f.write("Hello, World!")
# Output:
# Opening test.txt
# Closing test.txt

# Timer context manager
import time

class Timer:
    def __init__(self, description="Operation"):
        self.description = description
    
    def __enter__(self):
        self.start = time.time()
        return self
    
    def __exit__(self, *args):
        self.end = time.time()
        self.elapsed = self.end - self.start
        print(f"{self.description} took {self.elapsed:.4f} seconds")

with Timer("Data processing"):
    # Simulate some work
    time.sleep(0.5)
# Output: Data processing took 0.5012 seconds
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Forgetting `self`

```python
# Wrong
class Example:
    def method():  # Missing self!
        print("Hello")

# obj.method()  # TypeError: method() takes 0 arguments but 1 was given

# Correct
class Example:
    def method(self):
        print("Hello")
```

### ❌ Mistake 2: Mutable Default Arguments in __init__

```python
# Wrong
class Student:
    def __init__(self, name, grades=[]):
        self.name = name
        self.grades = grades  # Same list for all!

s1 = Student("Rahul")
s2 = Student("Priya")
s1.grades.append(90)
print(s2.grades)  # [90] - Bug!

# Correct
class Student:
    def __init__(self, name, grades=None):
        self.name = name
        self.grades = grades if grades is not None else []
```

### ❌ Mistake 3: Modifying Class Attribute via Instance

```python
# Confusing behavior
class Counter:
    count = 0

c1 = Counter()
c2 = Counter()

# This creates instance attribute, doesn't modify class attribute!
c1.count = 5
print(c1.count)      # 5 (instance)
print(c2.count)      # 0 (class)
print(Counter.count) # 0 (class)

# Correct way to modify class attribute
Counter.count = 10
print(c2.count)      # 10
```

### ❌ Mistake 4: Not Calling super().__init__()

```python
# Wrong
class Child(Parent):
    def __init__(self, name, age):
        # Forgot to call parent __init__!
        self.age = age
        # Parent attributes not initialized!

# Correct
class Child(Parent):
    def __init__(self, name, age):
        super().__init__(name)  # Initialize parent
        self.age = age
```

### ❌ Mistake 5: Circular References

```python
# Problem: Objects referencing each other
class A:
    def __init__(self, b_instance):
        self.b = b_instance

class B:
    def __init__(self, a_instance):
        self.a = a_instance

# Can cause memory issues
# Use weakref for one-way references
import weakref

class A:
    def __init__(self, b_instance):
        self.b = weakref.ref(b_instance)  # Weak reference
```

---

## 💡 Pro Tips (Industry Insights)

### 🔥 Tip 1: Use dataclasses (Python 3.7+)

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class Product:
    name: str
    price: float
    quantity: int = 0
    tags: List[str] = field(default_factory=list)
    
    @property
    def total_value(self):
        return self.price * self.quantity

# Automatic __init__, __repr__, __eq__
p1 = Product("Laptop", 50000, 5)
p2 = Product("Laptop", 50000, 5)

print(p1)           # Product(name='Laptop', price=50000, quantity=5, tags=[])
print(p1 == p2)     # True
print(p1.total_value)  # 250000

# Frozen (immutable) dataclass
@dataclass(frozen=True)
class Point:
    x: float
    y: float

point = Point(1.0, 2.0)
# point.x = 3.0  # FrozenInstanceError
```

### 🔥 Tip 2: Use __slots__ for Memory Optimization

```python
# Normal class
class NormalPoint:
    def __init__(self, x, y):
        self.x = x
        self.y = y

# Class with __slots__ (no __dict__)
class SlottedPoint:
    __slots__ = ['x', 'y']
    
    def __init__(self, x, y):
        self.x = x
        self.y = y

import sys
normal = NormalPoint(1, 2)
slotted = SlottedPoint(1, 2)

print(sys.getsizeof(normal.__dict__))  # ~104 bytes
# slotted has no __dict__, saves memory

# Cannot add new attributes with __slots__
# slotted.z = 3  # AttributeError
```

### 🔥 Tip 3: Use classmethod for Factory Pattern

```python
class Date:
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day
    
    @classmethod
    def from_string(cls, date_string):
        """Create Date from string 'YYYY-MM-DD'"""
        year, month, day = map(int, date_string.split('-'))
        return cls(year, month, day)
    
    @classmethod
    def today(cls):
        """Create Date for today"""
        import datetime
        t = datetime.date.today()
        return cls(t.year, t.month, t.day)
    
    def __str__(self):
        return f"{self.year}-{self.month:02d}-{self.day:02d}"

# Multiple ways to create Date
d1 = Date(2024, 1, 15)
d2 = Date.from_string("2024-01-15")
d3 = Date.today()

print(d1)  # 2024-01-15
```

### 🔥 Tip 4: Composition Over Inheritance

```python
# Instead of deep inheritance hierarchies, use composition

# Bad: Deep inheritance
class Animal: pass
class Mammal(Animal): pass
class Dog(Mammal): pass
class GermanShepherd(Dog): pass

# Good: Composition
class BarkBehavior:
    def bark(self):
        return "Woof!"

class FetchBehavior:
    def fetch(self):
        return "Fetching..."

class Dog:
    def __init__(self, name):
        self.name = name
        self.bark_behavior = BarkBehavior()
        self.fetch_behavior = FetchBehavior()
    
    def bark(self):
        return self.bark_behavior.bark()
    
    def fetch(self):
        return self.fetch_behavior.fetch()
```

### 🔥 Tip 5: Use typing for Type Hints

```python
from typing import Optional, List, Dict, Union, Callable
from dataclasses import dataclass

@dataclass
class User:
    id: int
    name: str
    email: Optional[str] = None
    roles: List[str] = None
    
    def __post_init__(self):
        if self.roles is None:
            self.roles = []

class UserService:
    def __init__(self):
        self.users: Dict[int, User] = {}
    
    def add_user(self, user: User) -> bool:
        if user.id in self.users:
            return False
        self.users[user.id] = user
        return True
    
    def get_user(self, user_id: int) -> Optional[User]:
        return self.users.get(user_id)
    
    def find_users(
        self, 
        filter_func: Callable[[User], bool]
    ) -> List[User]:
        return [u for u in self.users.values() if filter_func(u)]
```

---

## 🔍 Real-world Use Cases

### Use Case 1: E-commerce Product System

```python
from dataclasses import dataclass, field
from typing import List, Optional
from datetime import datetime
from abc import ABC, abstractmethod

@dataclass
class Product:
    id: int
    name: str
    price: float
    quantity: int
    category: str
    created_at: datetime = field(default_factory=datetime.now)
    
    def is_available(self) -> bool:
        return self.quantity > 0
    
    def apply_discount(self, percent: float) -> float:
        return self.price * (1 - percent / 100)

class DiscountStrategy(ABC):
    @abstractmethod
    def calculate(self, price: float) -> float:
        pass

class PercentageDiscount(DiscountStrategy):
    def __init__(self, percent: float):
        self.percent = percent
    
    def calculate(self, price: float) -> float:
        return price * (1 - self.percent / 100)

class FixedDiscount(DiscountStrategy):
    def __init__(self, amount: float):
        self.amount = amount
    
    def calculate(self, price: float) -> float:
        return max(0, price - self.amount)

class ShoppingCart:
    def __init__(self):
        self.items: List[tuple[Product, int]] = []
        self.discount_strategy: Optional[DiscountStrategy] = None
    
    def add_item(self, product: Product, quantity: int = 1):
        if product.is_available() and quantity <= product.quantity:
            self.items.append((product, quantity))
            return True
        return False
    
    def set_discount(self, strategy: DiscountStrategy):
        self.discount_strategy = strategy
    
    @property
    def subtotal(self) -> float:
        return sum(p.price * q for p, q in self.items)
    
    @property
    def total(self) -> float:
        subtotal = self.subtotal
        if self.discount_strategy:
            return self.discount_strategy.calculate(subtotal)
        return subtotal

# Usage
laptop = Product(1, "Laptop", 50000, 10, "Electronics")
phone = Product(2, "Phone", 30000, 5, "Electronics")

cart = ShoppingCart()
cart.add_item(laptop, 1)
cart.add_item(phone, 2)

print(f"Subtotal: ₹{cart.subtotal}")  # ₹110000

cart.set_discount(PercentageDiscount(10))
print(f"Total (10% off): ₹{cart.total}")  # ₹99000
```

### Use Case 2: Event System with Observer Pattern

```python
from typing import List, Callable, Any
from dataclasses import dataclass, field

@dataclass
class Event:
    name: str
    data: Any = None

class EventEmitter:
    """Observable class that emits events"""
    
    def __init__(self):
        self._listeners: dict[str, List[Callable]] = {}
    
    def on(self, event_name: str, callback: Callable):
        """Subscribe to an event"""
        if event_name not in self._listeners:
            self._listeners[event_name] = []
        self._listeners[event_name].append(callback)
        return self
    
    def off(self, event_name: str, callback: Callable = None):
        """Unsubscribe from an event"""
        if callback is None:
            self._listeners.pop(event_name, None)
        elif event_name in self._listeners:
            self._listeners[event_name].remove(callback)
        return self
    
    def emit(self, event_name: str, data: Any = None):
        """Emit an event"""
        event = Event(event_name, data)
        for callback in self._listeners.get(event_name, []):
            callback(event)

class User(EventEmitter):
    def __init__(self, name: str):
        super().__init__()
        self.name = name
        self._logged_in = False
    
    def login(self):
        self._logged_in = True
        self.emit('login', {'user': self.name})
    
    def logout(self):
        self._logged_in = False
        self.emit('logout', {'user': self.name})

# Usage
def log_activity(event: Event):
    print(f"Activity: {event.name} - {event.data}")

def send_notification(event: Event):
    print(f"Notification: User {event.data['user']} logged in!")

user = User("Rahul")
user.on('login', log_activity)
user.on('login', send_notification)
user.on('logout', log_activity)

user.login()
# Output:
# Activity: login - {'user': 'Rahul'}
# Notification: User Rahul logged in!

user.logout()
# Output:
# Activity: logout - {'user': 'Rahul'}
```

### Use Case 3: REST API Resource Classes

```python
from abc import ABC, abstractmethod
from typing import Dict, Any, Optional
from dataclasses import dataclass, asdict
import json

@dataclass
class APIResponse:
    success: bool
    data: Any = None
    error: str = None
    
    def to_json(self) -> str:
        return json.dumps(asdict(self))

class Resource(ABC):
    """Base class for REST resources"""
    
    @abstractmethod
    def get(self, id: int) -> APIResponse:
        pass
    
    @abstractmethod
    def create(self, data: Dict) -> APIResponse:
        pass
    
    @abstractmethod
    def update(self, id: int, data: Dict) -> APIResponse:
        pass
    
    @abstractmethod
    def delete(self, id: int) -> APIResponse:
        pass

class UserResource(Resource):
    def __init__(self):
        self._users: Dict[int, Dict] = {}
        self._next_id = 1
    
    def get(self, id: int) -> APIResponse:
        user = self._users.get(id)
        if user:
            return APIResponse(True, data=user)
        return APIResponse(False, error="User not found")
    
    def create(self, data: Dict) -> APIResponse:
        user_id = self._next_id
        self._next_id += 1
        
        user = {"id": user_id, **data}
        self._users[user_id] = user
        return APIResponse(True, data=user)
    
    def update(self, id: int, data: Dict) -> APIResponse:
        if id not in self._users:
            return APIResponse(False, error="User not found")
        
        self._users[id].update(data)
        return APIResponse(True, data=self._users[id])
    
    def delete(self, id: int) -> APIResponse:
        if id in self._users:
            del self._users[id]
            return APIResponse(True)
        return APIResponse(False, error="User not found")

# Usage
users = UserResource()
print(users.create({"name": "Rahul", "email": "rahul@example.com"}).to_json())
print(users.get(1).to_json())
print(users.update(1, {"name": "Rahul Sharma"}).to_json())
```

---

## 🧪 Practice Questions

### 🟢 Easy

1. **Basic Class**: Create a `Book` class with title, author, pages. Add method to display info.

2. **Rectangle Class**: Create with width, height. Add methods for area, perimeter.

3. **Counter Class**: Class with increment, decrement, reset methods.

4. **Student Class**: Store name, roll_no, marks. Add method to check pass/fail.

### 🟡 Medium

5. **Bank Account**: Implement with deposit, withdraw, transfer methods. Handle insufficient balance.

6. **Inheritance**: Create `Shape` base class. Derive `Circle`, `Rectangle`, `Triangle`.

7. **Library System**: Book, Member, Library classes with borrow/return functionality.

8. **Deck of Cards**: Card class, Deck class with shuffle, draw methods.

### 🔴 Hard

9. **Linked List**: Implement singly linked list with insert, delete, search, reverse.

10. **Binary Tree**: Implement with insert, search, traversals (inorder, preorder, postorder).

11. **LRU Cache**: Implement using OOP principles with get, put operations.

---

## 🚀 Mini Projects / Tasks

### Project 1: Employee Management System

**Problem Statement:** Create an OOP-based employee management system.

**Features:**
- Employee class with different types (Manager, Developer, Intern)
- Department class
- Salary calculation with bonuses
- Performance reviews
- Generate reports

### Project 2: Simple Game Engine

**Problem Statement:** Create a basic game framework using OOP.

**Features:**
- GameObject base class
- Player, Enemy, Collectible classes
- Position, Movement components
- Collision detection
- Score tracking

---

## 💼 Interview Insight

### 🔥 Most Asked Questions

1. **Q: OOP ke 4 pillars kya hain?**
   
   **A:**
   - **Encapsulation**: Data hiding using private/protected
   - **Inheritance**: Child class inherits parent features
   - **Polymorphism**: Same interface, different implementations
   - **Abstraction**: Hide complexity, show only necessary

2. **Q: `@classmethod` vs `@staticmethod` difference?**
   
   **A:**
   - `@classmethod`: Gets class (cls) as first argument, can access/modify class state
   - `@staticmethod`: No special first argument, just a function inside class

3. **Q: Multiple inheritance mein diamond problem kya hai?**
   
   **A:** When a class inherits from two classes that have common parent, ambiguity arises which parent's method to use. Python uses MRO (Method Resolution Order) with C3 linearization to resolve.

4. **Q: `super()` kya karta hai?**
   
   **A:** Returns a proxy object that delegates method calls to parent class. Used to call parent's methods without explicitly naming the parent class.

5. **Q: Abstract class vs Interface difference?**
   
   **A:** Python doesn't have interfaces like Java. ABC (Abstract Base Class) serves both purposes:
   - Can have abstract methods (must be implemented)
   - Can have concrete methods (optional to override)

6. **Q: What are magic methods?**
   
   **A:** Special methods with double underscores (`__init__`, `__str__`, `__add__`, etc.) that Python calls implicitly for certain operations.

---

## ⚡ Shortcut Tricks

```python
# Trick 1: Quick class with __slots__
class Point:
    __slots__ = ('x', 'y')
    def __init__(self, x, y):
        self.x, self.y = x, y

# Trick 2: Dataclass for quick data classes
from dataclasses import dataclass
@dataclass
class User:
    name: str
    age: int

# Trick 3: Named tuple for simple immutable classes
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])

# Trick 4: Property for computed attributes
class Circle:
    def __init__(self, r):
        self.radius = r
    
    @property
    def area(self):
        return 3.14 * self.radius ** 2

# Trick 5: __call__ to make objects callable
class Multiplier:
    def __init__(self, factor):
        self.factor = factor
    def __call__(self, x):
        return x * self.factor

double = Multiplier(2)
print(double(5))  # 10

# Trick 6: Context manager with __enter__/__exit__
class Timer:
    def __enter__(self):
        import time
        self.start = time.time()
        return self
    def __exit__(self, *args):
        print(f"Elapsed: {time.time() - self.start:.2f}s")
```

---

## 📝 Summary

Is module mein humne seekha:

- ✅ Classes aur Objects basics
- ✅ Constructor (`__init__`) aur `self`
- ✅ Instance vs Class attributes
- ✅ Instance, Class, aur Static methods
- ✅ Encapsulation (public, protected, private)
- ✅ Properties aur Descriptors
- ✅ Inheritance (single, multiple, MRO)
- ✅ Polymorphism aur Abstract classes
- ✅ Magic/Dunder methods
- ✅ Composition vs Inheritance
- ✅ Metaclasses basics
- ✅ Dataclasses aur __slots__

---

**Next Module:** [06_File_Handling_and_Exception.md](./06_File_Handling_and_Exception.md) - File Operations aur Error Handling

---

<div align="center">

**Made with ❤️ for Python Learners**

[⬆️ Back to Top](#-05_object_oriented_programmingmd)

</div>
