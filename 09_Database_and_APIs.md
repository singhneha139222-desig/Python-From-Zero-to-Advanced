# 📘 09_Database_and_APIs.md

## 📌 Introduction

**Database** aur **APIs** modern applications ki backbone hain!

- 🗄️ **Database** = Data storage and retrieval
- 🔌 **API** = Application Programming Interface (programs ki baat-cheet)

**Types of Databases:**
| Type | Examples | Use Case |
|------|----------|----------|
| Relational (SQL) | SQLite, PostgreSQL, MySQL | Structured data, transactions |
| NoSQL | MongoDB, Redis, Cassandra | Flexible schema, high scale |
| In-Memory | Redis, Memcached | Caching, real-time |

**Types of APIs:**
| Type | Description |
|------|-------------|
| REST | Most common, HTTP-based |
| GraphQL | Query language for APIs |
| WebSocket | Real-time, bidirectional |
| gRPC | High-performance RPC |

---

## 🧠 Core Concepts

### Database Basics

```
CRUD Operations:
- C = Create (INSERT)
- R = Read (SELECT)
- U = Update (UPDATE)
- D = Delete (DELETE)

SQL vs ORM:
- SQL: Direct database queries
- ORM: Python objects ↔ Database tables
```

### API Architecture

```
Client → Request → Server → Database
   ←    Response  ←

REST Principles:
- Stateless
- Client-Server
- Cacheable
- Uniform Interface
```

---

## 💻 SQL DATABASES

### 🟢 SQLite - Built-in Database

```python
import sqlite3
from contextlib import contextmanager

# Context manager for database connection
@contextmanager
def get_connection(db_name="app.db"):
    conn = sqlite3.connect(db_name)
    conn.row_factory = sqlite3.Row  # Access columns by name
    try:
        yield conn
        conn.commit()
    except Exception:
        conn.rollback()
        raise
    finally:
        conn.close()

# Create table
def create_tables():
    with get_connection() as conn:
        conn.execute("""
            CREATE TABLE IF NOT EXISTS users (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                name TEXT NOT NULL,
                email TEXT UNIQUE NOT NULL,
                age INTEGER,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
            )
        """)
        
        conn.execute("""
            CREATE TABLE IF NOT EXISTS posts (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                user_id INTEGER NOT NULL,
                title TEXT NOT NULL,
                content TEXT,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                FOREIGN KEY (user_id) REFERENCES users (id)
            )
        """)

# INSERT - Create
def create_user(name, email, age=None):
    with get_connection() as conn:
        cursor = conn.execute(
            "INSERT INTO users (name, email, age) VALUES (?, ?, ?)",
            (name, email, age)
        )
        return cursor.lastrowid

# SELECT - Read
def get_user(user_id):
    with get_connection() as conn:
        cursor = conn.execute(
            "SELECT * FROM users WHERE id = ?",
            (user_id,)
        )
        row = cursor.fetchone()
        return dict(row) if row else None

def get_all_users():
    with get_connection() as conn:
        cursor = conn.execute("SELECT * FROM users ORDER BY name")
        return [dict(row) for row in cursor.fetchall()]

def search_users(query):
    with get_connection() as conn:
        cursor = conn.execute(
            "SELECT * FROM users WHERE name LIKE ?",
            (f"%{query}%",)
        )
        return [dict(row) for row in cursor.fetchall()]

# UPDATE
def update_user(user_id, **kwargs):
    if not kwargs:
        return False
    
    fields = ", ".join(f"{k} = ?" for k in kwargs.keys())
    values = list(kwargs.values()) + [user_id]
    
    with get_connection() as conn:
        cursor = conn.execute(
            f"UPDATE users SET {fields} WHERE id = ?",
            values
        )
        return cursor.rowcount > 0

# DELETE
def delete_user(user_id):
    with get_connection() as conn:
        cursor = conn.execute(
            "DELETE FROM users WHERE id = ?",
            (user_id,)
        )
        return cursor.rowcount > 0

# Joins
def get_user_with_posts(user_id):
    with get_connection() as conn:
        cursor = conn.execute("""
            SELECT u.*, p.id as post_id, p.title, p.content
            FROM users u
            LEFT JOIN posts p ON u.id = p.user_id
            WHERE u.id = ?
        """, (user_id,))
        return [dict(row) for row in cursor.fetchall()]

# Aggregations
def get_user_stats():
    with get_connection() as conn:
        cursor = conn.execute("""
            SELECT 
                COUNT(*) as total_users,
                AVG(age) as avg_age,
                MIN(age) as min_age,
                MAX(age) as max_age
            FROM users
        """)
        return dict(cursor.fetchone())

# Transaction example
def transfer_data(source_id, target_id):
    with get_connection() as conn:
        # Both operations succeed or both fail
        conn.execute(
            "UPDATE accounts SET balance = balance - 100 WHERE id = ?",
            (source_id,)
        )
        conn.execute(
            "UPDATE accounts SET balance = balance + 100 WHERE id = ?",
            (target_id,)
        )

# Usage
create_tables()
user_id = create_user("Rahul", "rahul@example.com", 25)
print(get_user(user_id))
update_user(user_id, name="Rahul Sharma", age=26)
print(get_all_users())
```

### 🟡 PostgreSQL/MySQL with psycopg2/mysql-connector

```python
import psycopg2
from psycopg2.extras import RealDictCursor
from contextlib import contextmanager

# PostgreSQL connection
DATABASE_URL = "postgresql://user:password@localhost:5432/mydb"

@contextmanager
def get_pg_connection():
    conn = psycopg2.connect(DATABASE_URL)
    try:
        yield conn
        conn.commit()
    except Exception:
        conn.rollback()
        raise
    finally:
        conn.close()

def get_users_pg():
    with get_pg_connection() as conn:
        with conn.cursor(cursor_factory=RealDictCursor) as cur:
            cur.execute("SELECT * FROM users")
            return cur.fetchall()

# MySQL connection
import mysql.connector

def get_mysql_connection():
    return mysql.connector.connect(
        host="localhost",
        user="root",
        password="password",
        database="mydb"
    )

# Connection pooling (production use)
from psycopg2 import pool

connection_pool = pool.ThreadedConnectionPool(
    minconn=1,
    maxconn=10,
    dsn=DATABASE_URL
)

def get_pooled_connection():
    return connection_pool.getconn()

def return_connection(conn):
    connection_pool.putconn(conn)
```

### 🔴 SQLAlchemy - ORM

```python
from sqlalchemy import create_engine, Column, Integer, String, DateTime, ForeignKey, Text
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship
from datetime import datetime

# Setup
DATABASE_URL = "sqlite:///app.db"
# DATABASE_URL = "postgresql://user:pass@localhost/db"

engine = create_engine(DATABASE_URL, echo=True)  # echo=True shows SQL
Base = declarative_base()
Session = sessionmaker(bind=engine)

# Models
class User(Base):
    __tablename__ = "users"
    
    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)
    email = Column(String(100), unique=True, nullable=False)
    age = Column(Integer)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    # Relationship
    posts = relationship("Post", back_populates="author", cascade="all, delete-orphan")
    
    def __repr__(self):
        return f"<User(name='{self.name}', email='{self.email}')>"
    
    def to_dict(self):
        return {
            "id": self.id,
            "name": self.name,
            "email": self.email,
            "age": self.age,
            "created_at": self.created_at.isoformat() if self.created_at else None
        }

class Post(Base):
    __tablename__ = "posts"
    
    id = Column(Integer, primary_key=True)
    title = Column(String(200), nullable=False)
    content = Column(Text)
    user_id = Column(Integer, ForeignKey("users.id"), nullable=False)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    author = relationship("User", back_populates="posts")

# Create tables
Base.metadata.create_all(engine)

# CRUD Operations
class UserRepository:
    def __init__(self):
        self.session = Session()
    
    def create(self, name, email, age=None):
        user = User(name=name, email=email, age=age)
        self.session.add(user)
        self.session.commit()
        return user
    
    def get_by_id(self, user_id):
        return self.session.query(User).filter(User.id == user_id).first()
    
    def get_by_email(self, email):
        return self.session.query(User).filter(User.email == email).first()
    
    def get_all(self):
        return self.session.query(User).all()
    
    def search(self, query):
        return self.session.query(User).filter(
            User.name.ilike(f"%{query}%")
        ).all()
    
    def update(self, user_id, **kwargs):
        user = self.get_by_id(user_id)
        if user:
            for key, value in kwargs.items():
                setattr(user, key, value)
            self.session.commit()
        return user
    
    def delete(self, user_id):
        user = self.get_by_id(user_id)
        if user:
            self.session.delete(user)
            self.session.commit()
            return True
        return False
    
    def close(self):
        self.session.close()

# Advanced queries
def advanced_queries():
    session = Session()
    
    # Filter with multiple conditions
    users = session.query(User).filter(
        User.age >= 18,
        User.name.like("R%")
    ).all()
    
    # Order by
    users = session.query(User).order_by(User.name.desc()).all()
    
    # Limit and offset (pagination)
    users = session.query(User).limit(10).offset(20).all()
    
    # Count
    count = session.query(User).count()
    
    # Group by with aggregation
    from sqlalchemy import func
    stats = session.query(
        User.age,
        func.count(User.id).label("count")
    ).group_by(User.age).all()
    
    # Join
    results = session.query(User, Post).join(Post).all()
    
    # Subquery
    subquery = session.query(func.avg(User.age)).scalar_subquery()
    above_avg = session.query(User).filter(User.age > subquery).all()
    
    session.close()

# Context manager for sessions
from contextlib import contextmanager

@contextmanager
def get_session():
    session = Session()
    try:
        yield session
        session.commit()
    except Exception:
        session.rollback()
        raise
    finally:
        session.close()

# Usage with context manager
with get_session() as session:
    user = User(name="Test", email="test@example.com")
    session.add(user)
```

---

## 📡 NoSQL DATABASES

### MongoDB with PyMongo

```python
from pymongo import MongoClient
from bson import ObjectId
from datetime import datetime

# Connection
client = MongoClient("mongodb://localhost:27017/")
db = client["myapp"]
users_collection = db["users"]

# Insert
def create_user(name, email, age=None):
    user = {
        "name": name,
        "email": email,
        "age": age,
        "created_at": datetime.utcnow()
    }
    result = users_collection.insert_one(user)
    return str(result.inserted_id)

def create_many_users(users):
    result = users_collection.insert_many(users)
    return [str(id) for id in result.inserted_ids]

# Find
def get_user(user_id):
    return users_collection.find_one({"_id": ObjectId(user_id)})

def get_all_users():
    return list(users_collection.find())

def search_users(query):
    return list(users_collection.find({
        "name": {"$regex": query, "$options": "i"}  # Case-insensitive
    }))

def filter_users(min_age=None, max_age=None):
    query = {}
    if min_age:
        query["age"] = {"$gte": min_age}
    if max_age:
        query.setdefault("age", {})["$lte"] = max_age
    return list(users_collection.find(query))

# Update
def update_user(user_id, **updates):
    result = users_collection.update_one(
        {"_id": ObjectId(user_id)},
        {"$set": updates}
    )
    return result.modified_count > 0

def increment_age(user_id):
    users_collection.update_one(
        {"_id": ObjectId(user_id)},
        {"$inc": {"age": 1}}
    )

# Delete
def delete_user(user_id):
    result = users_collection.delete_one({"_id": ObjectId(user_id)})
    return result.deleted_count > 0

# Aggregation
def get_age_stats():
    pipeline = [
        {"$group": {
            "_id": None,
            "avg_age": {"$avg": "$age"},
            "min_age": {"$min": "$age"},
            "max_age": {"$max": "$age"},
            "total": {"$sum": 1}
        }}
    ]
    result = list(users_collection.aggregate(pipeline))
    return result[0] if result else None

def get_users_by_age_group():
    pipeline = [
        {"$bucket": {
            "groupBy": "$age",
            "boundaries": [0, 20, 30, 40, 50, 100],
            "default": "Other",
            "output": {
                "count": {"$sum": 1},
                "users": {"$push": "$name"}
            }
        }}
    ]
    return list(users_collection.aggregate(pipeline))

# Indexing
users_collection.create_index("email", unique=True)
users_collection.create_index([("name", 1), ("age", -1)])

# Text search
users_collection.create_index([("name", "text"), ("bio", "text")])
def text_search(query):
    return list(users_collection.find({"$text": {"$search": query}}))
```

### Redis with redis-py

```python
import redis
import json
from datetime import timedelta

# Connection
r = redis.Redis(host='localhost', port=6379, db=0, decode_responses=True)

# String operations
r.set("name", "Rahul")
name = r.get("name")
print(name)  # Rahul

# With expiration
r.setex("session_token", timedelta(hours=1), "abc123")

# Increment/Decrement
r.set("counter", 0)
r.incr("counter")      # 1
r.incrby("counter", 5) # 6
r.decr("counter")      # 5

# Hash (like dict)
r.hset("user:1", mapping={"name": "Rahul", "email": "rahul@example.com", "age": "25"})
user = r.hgetall("user:1")
print(user)  # {'name': 'Rahul', 'email': 'rahul@example.com', 'age': '25'}

r.hget("user:1", "name")     # Get single field
r.hincrby("user:1", "age", 1) # Increment

# List (queue)
r.rpush("tasks", "task1", "task2", "task3")  # Push right
task = r.lpop("tasks")                        # Pop left
all_tasks = r.lrange("tasks", 0, -1)         # Get all

# Set
r.sadd("tags", "python", "redis", "database")
tags = r.smembers("tags")
r.sismember("tags", "python")  # True

# Sorted Set (leaderboard)
r.zadd("leaderboard", {"player1": 100, "player2": 85, "player3": 92})
top_players = r.zrevrange("leaderboard", 0, 2, withscores=True)
print(top_players)  # [('player1', 100.0), ('player3', 92.0), ('player2', 85.0)]

# Caching pattern
def get_user_cached(user_id):
    cache_key = f"user:{user_id}"
    
    # Check cache
    cached = r.get(cache_key)
    if cached:
        return json.loads(cached)
    
    # Fetch from database
    user = fetch_user_from_db(user_id)  # Your DB function
    
    # Store in cache with expiration
    r.setex(cache_key, timedelta(minutes=10), json.dumps(user))
    
    return user

# Pub/Sub
def publisher():
    r.publish("notifications", "New message!")

def subscriber():
    pubsub = r.pubsub()
    pubsub.subscribe("notifications")
    for message in pubsub.listen():
        if message["type"] == "message":
            print(f"Received: {message['data']}")

# Pipeline (batch operations)
pipe = r.pipeline()
pipe.set("key1", "value1")
pipe.set("key2", "value2")
pipe.get("key1")
results = pipe.execute()
```

---

## 🌐 REST API DEVELOPMENT

### FastAPI Complete Example

```python
from fastapi import FastAPI, HTTPException, Depends, Query, Path, Body, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from pydantic import BaseModel, EmailStr, Field
from typing import Optional, List
from datetime import datetime, timedelta
import jwt
from passlib.context import CryptContext

# Initialize app
app = FastAPI(
    title="User Management API",
    description="A complete CRUD API with authentication",
    version="1.0.0"
)

# Security
SECRET_KEY = "your-secret-key-here"
ALGORITHM = "HS256"
pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

# Pydantic Models
class UserBase(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)
    email: EmailStr
    age: Optional[int] = Field(None, ge=0, le=150)

class UserCreate(UserBase):
    password: str = Field(..., min_length=8)

class User(UserBase):
    id: int
    created_at: datetime
    
    class Config:
        from_attributes = True

class UserUpdate(BaseModel):
    name: Optional[str] = None
    email: Optional[EmailStr] = None
    age: Optional[int] = None

class Token(BaseModel):
    access_token: str
    token_type: str

# Fake database
users_db = {}
user_id_counter = 0

# Helper functions
def get_password_hash(password):
    return pwd_context.hash(password)

def verify_password(plain_password, hashed_password):
    return pwd_context.verify(plain_password, hashed_password)

def create_access_token(data: dict, expires_delta: timedelta = None):
    to_encode = data.copy()
    expire = datetime.utcnow() + (expires_delta or timedelta(hours=1))
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)

async def get_current_user(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        email = payload.get("sub")
        if email is None:
            raise HTTPException(status_code=401, detail="Invalid token")
    except jwt.PyJWTError:
        raise HTTPException(status_code=401, detail="Invalid token")
    
    for user in users_db.values():
        if user["email"] == email:
            return user
    raise HTTPException(status_code=404, detail="User not found")

# Routes
@app.post("/token", response_model=Token)
async def login(form_data: OAuth2PasswordRequestForm = Depends()):
    """Authenticate and get access token"""
    for user in users_db.values():
        if user["email"] == form_data.username:
            if verify_password(form_data.password, user["password"]):
                token = create_access_token({"sub": user["email"]})
                return {"access_token": token, "token_type": "bearer"}
    raise HTTPException(status_code=401, detail="Invalid credentials")

@app.get("/users", response_model=List[User])
async def get_users(
    skip: int = Query(0, ge=0),
    limit: int = Query(10, ge=1, le=100),
    search: Optional[str] = None
):
    """Get all users with pagination and search"""
    users = list(users_db.values())
    
    if search:
        users = [u for u in users if search.lower() in u["name"].lower()]
    
    return users[skip:skip + limit]

@app.get("/users/{user_id}", response_model=User)
async def get_user(user_id: int = Path(..., gt=0)):
    """Get user by ID"""
    if user_id not in users_db:
        raise HTTPException(status_code=404, detail="User not found")
    return users_db[user_id]

@app.post("/users", response_model=User, status_code=status.HTTP_201_CREATED)
async def create_user(user: UserCreate):
    """Create a new user"""
    global user_id_counter
    
    # Check if email exists
    for u in users_db.values():
        if u["email"] == user.email:
            raise HTTPException(status_code=400, detail="Email already registered")
    
    user_id_counter += 1
    user_data = {
        "id": user_id_counter,
        **user.dict(),
        "password": get_password_hash(user.password),
        "created_at": datetime.utcnow()
    }
    users_db[user_id_counter] = user_data
    return user_data

@app.put("/users/{user_id}", response_model=User)
async def update_user(
    user_id: int,
    user_update: UserUpdate,
    current_user: dict = Depends(get_current_user)
):
    """Update user (authenticated)"""
    if user_id not in users_db:
        raise HTTPException(status_code=404, detail="User not found")
    
    update_data = user_update.dict(exclude_unset=True)
    users_db[user_id].update(update_data)
    return users_db[user_id]

@app.delete("/users/{user_id}", status_code=status.HTTP_204_NO_CONTENT)
async def delete_user(
    user_id: int,
    current_user: dict = Depends(get_current_user)
):
    """Delete user (authenticated)"""
    if user_id not in users_db:
        raise HTTPException(status_code=404, detail="User not found")
    del users_db[user_id]

# Run with: uvicorn main:app --reload
```

### API Client Example

```python
import requests
from typing import Optional, List
import json

class APIClient:
    """Client for interacting with the User Management API"""
    
    def __init__(self, base_url: str):
        self.base_url = base_url.rstrip("/")
        self.token = None
    
    def _get_headers(self):
        headers = {"Content-Type": "application/json"}
        if self.token:
            headers["Authorization"] = f"Bearer {self.token}"
        return headers
    
    def login(self, email: str, password: str) -> bool:
        """Authenticate and store token"""
        response = requests.post(
            f"{self.base_url}/token",
            data={"username": email, "password": password}
        )
        if response.status_code == 200:
            self.token = response.json()["access_token"]
            return True
        return False
    
    def get_users(self, skip: int = 0, limit: int = 10, search: str = None) -> List[dict]:
        """Get users with pagination"""
        params = {"skip": skip, "limit": limit}
        if search:
            params["search"] = search
        
        response = requests.get(
            f"{self.base_url}/users",
            params=params,
            headers=self._get_headers()
        )
        response.raise_for_status()
        return response.json()
    
    def get_user(self, user_id: int) -> Optional[dict]:
        """Get user by ID"""
        response = requests.get(
            f"{self.base_url}/users/{user_id}",
            headers=self._get_headers()
        )
        if response.status_code == 404:
            return None
        response.raise_for_status()
        return response.json()
    
    def create_user(self, name: str, email: str, password: str, age: int = None) -> dict:
        """Create new user"""
        data = {"name": name, "email": email, "password": password}
        if age:
            data["age"] = age
        
        response = requests.post(
            f"{self.base_url}/users",
            json=data,
            headers=self._get_headers()
        )
        response.raise_for_status()
        return response.json()
    
    def update_user(self, user_id: int, **updates) -> dict:
        """Update user"""
        response = requests.put(
            f"{self.base_url}/users/{user_id}",
            json=updates,
            headers=self._get_headers()
        )
        response.raise_for_status()
        return response.json()
    
    def delete_user(self, user_id: int) -> bool:
        """Delete user"""
        response = requests.delete(
            f"{self.base_url}/users/{user_id}",
            headers=self._get_headers()
        )
        return response.status_code == 204

# Usage
client = APIClient("http://localhost:8000")

# Create user
user = client.create_user("Rahul", "rahul@example.com", "password123", 25)
print(f"Created user: {user}")

# Login
client.login("rahul@example.com", "password123")

# Get users
users = client.get_users(limit=5)
print(f"Users: {users}")

# Update user
updated = client.update_user(user["id"], name="Rahul Sharma")
print(f"Updated: {updated}")
```

---

## 🔄 Database Migrations (Alembic)

```python
# alembic.ini and migrations setup
# pip install alembic
# alembic init migrations

# env.py (in migrations folder)
from alembic import context
from sqlalchemy import engine_from_config
from myapp.models import Base  # Your models

target_metadata = Base.metadata

# Create migration
# alembic revision --autogenerate -m "Create users table"

# Run migrations
# alembic upgrade head

# Rollback
# alembic downgrade -1

# Example migration file (versions/xxxx_create_users_table.py)
"""Create users table

Revision ID: abcd1234
"""
from alembic import op
import sqlalchemy as sa

def upgrade():
    op.create_table(
        'users',
        sa.Column('id', sa.Integer(), nullable=False),
        sa.Column('name', sa.String(100), nullable=False),
        sa.Column('email', sa.String(100), nullable=False),
        sa.PrimaryKeyConstraint('id'),
        sa.UniqueConstraint('email')
    )

def downgrade():
    op.drop_table('users')
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: SQL Injection

```python
# Wrong - Vulnerable to SQL injection
user_input = "'; DROP TABLE users; --"
query = f"SELECT * FROM users WHERE name = '{user_input}'"
cursor.execute(query)  # Dangerous!

# Correct - Use parameterized queries
cursor.execute("SELECT * FROM users WHERE name = ?", (user_input,))
```

### ❌ Mistake 2: N+1 Query Problem

```python
# Wrong - N+1 queries
users = session.query(User).all()
for user in users:
    print(user.posts)  # Each access triggers a query!

# Correct - Eager loading
from sqlalchemy.orm import joinedload
users = session.query(User).options(joinedload(User.posts)).all()
```

### ❌ Mistake 3: Not Closing Connections

```python
# Wrong
conn = psycopg2.connect(...)
cursor = conn.cursor()
cursor.execute("SELECT * FROM users")
# Connection never closed!

# Correct - Use context manager
with psycopg2.connect(...) as conn:
    with conn.cursor() as cursor:
        cursor.execute("SELECT * FROM users")
```

### ❌ Mistake 4: Hardcoded Credentials

```python
# Wrong
DATABASE_URL = "postgresql://admin:password123@localhost/mydb"

# Correct - Use environment variables
import os
DATABASE_URL = os.environ.get("DATABASE_URL")
```

---

## 💡 Pro Tips

### 🔥 Tip 1: Connection Pooling

```python
from sqlalchemy import create_engine
from sqlalchemy.pool import QueuePool

engine = create_engine(
    DATABASE_URL,
    poolclass=QueuePool,
    pool_size=10,
    max_overflow=20,
    pool_timeout=30,
    pool_recycle=1800
)
```

### 🔥 Tip 2: Bulk Operations

```python
# Instead of inserting one by one
for user in users:
    session.add(User(**user))
session.commit()

# Use bulk insert
session.bulk_insert_mappings(User, users)
session.commit()
```

### 🔥 Tip 3: Indexing Strategy

```python
# Add indexes for frequently queried columns
class User(Base):
    __tablename__ = "users"
    
    id = Column(Integer, primary_key=True)
    email = Column(String(100), unique=True, index=True)  # Index!
    name = Column(String(100), index=True)  # Index!
    
    # Composite index
    __table_args__ = (
        Index('ix_user_name_email', 'name', 'email'),
    )
```

### 🔥 Tip 4: Query Optimization

```python
# Use EXPLAIN to analyze queries
cursor.execute("EXPLAIN ANALYZE SELECT * FROM users WHERE age > 25")
print(cursor.fetchall())

# Select only needed columns
users = session.query(User.id, User.name).all()  # Not User.*
```

---

## 🧪 Practice Questions

1. **CRUD API**: Build a complete CRUD API for a blog system (posts, comments).

2. **Database Migration**: Create migrations for adding new fields to existing tables.

3. **Caching Layer**: Implement Redis caching for database queries.

4. **API Rate Limiting**: Add rate limiting to your FastAPI endpoints.

5. **Database Backup**: Write a script to backup and restore database.

---

## 🚀 Mini Projects

### Project 1: Todo API with Authentication

Build a full-featured Todo API:
- User registration and login
- JWT authentication
- CRUD for todos
- Categories and tags
- Due dates and reminders

### Project 2: E-commerce Backend

Build an e-commerce API:
- Products, categories
- User authentication
- Shopping cart
- Orders and payments (mock)
- Inventory management

---

## 💼 Interview Insight

### 🔥 Most Asked Questions

1. **Q: SQL vs NoSQL kab use karein?**
   **A:** SQL: Structured data, ACID transactions, complex queries. NoSQL: Flexible schema, high scalability, simple queries.

2. **Q: ORM ke advantages/disadvantages?**
   **A:** Advantages: Abstraction, security, portability. Disadvantages: Performance overhead, learning curve, less control.

3. **Q: Database indexing kya hai?**
   **A:** Index data structure hai jo queries ko fast karta hai. Tradeoff: Insert/Update slow ho jate hain.

4. **Q: REST API design best practices?**
   **A:** Use nouns for resources, proper HTTP methods, versioning, pagination, error handling.

---

## 📝 Summary

- ✅ SQLite, PostgreSQL, MySQL connections
- ✅ SQLAlchemy ORM
- ✅ MongoDB with PyMongo
- ✅ Redis caching
- ✅ FastAPI REST API development
- ✅ Authentication with JWT
- ✅ Database migrations
- ✅ Query optimization

---

**Next Module:** [10_Projects_and_Interview_Prep.md](./10_Projects_and_Interview_Prep.md) - Real Projects aur Interview Preparation

---

<div align="center">

**Made with ❤️ for Python Learners**

[⬆️ Back to Top](#-09_database_and_apismd)

</div>
