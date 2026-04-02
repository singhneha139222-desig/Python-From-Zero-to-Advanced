# 📘 10_Projects_and_Interview_Prep.md

## 📌 Introduction

Congratulations! 🎉 Tum Python ka theoretical knowledge complete kar chuke ho. Ab time hai:
- 🚀 **Real-world Projects** build karne ka
- 💼 **Interview Preparation** ka
- 🗺️ **Career Roadmap** follow karne ka

> 💡 **Important:** Practice without projects is incomplete. Projects hi tumhe job-ready banate hain!

---

## 🚀 REAL-WORLD PROJECTS

## 🟢 BEGINNER PROJECTS

### Project 1: Personal Expense Tracker CLI

**Problem Statement:** Command-line application to track daily expenses.

**Features:**
- Add expenses with category and amount
- View expenses by date/category
- Monthly summary reports
- Export to CSV
- Budget alerts

**Tech Stack:** Python, SQLite, argparse

```python
# expense_tracker.py
import sqlite3
import argparse
from datetime import datetime, timedelta
from collections import defaultdict
import csv

class ExpenseTracker:
    def __init__(self, db_name="expenses.db"):
        self.conn = sqlite3.connect(db_name)
        self.setup_database()
    
    def setup_database(self):
        self.conn.execute("""
            CREATE TABLE IF NOT EXISTS expenses (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                amount REAL NOT NULL,
                category TEXT NOT NULL,
                description TEXT,
                date DATE DEFAULT CURRENT_DATE,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
            )
        """)
        
        self.conn.execute("""
            CREATE TABLE IF NOT EXISTS budgets (
                category TEXT PRIMARY KEY,
                monthly_limit REAL NOT NULL
            )
        """)
        self.conn.commit()
    
    def add_expense(self, amount, category, description=""):
        self.conn.execute(
            "INSERT INTO expenses (amount, category, description) VALUES (?, ?, ?)",
            (amount, category.lower(), description)
        )
        self.conn.commit()
        print(f"✅ Added: ₹{amount} in {category}")
        self.check_budget(category)
    
    def get_expenses(self, days=30, category=None):
        query = """
            SELECT * FROM expenses 
            WHERE date >= date('now', ?)
        """
        params = [f'-{days} days']
        
        if category:
            query += " AND category = ?"
            params.append(category.lower())
        
        query += " ORDER BY date DESC"
        return self.conn.execute(query, params).fetchall()
    
    def get_summary(self, month=None):
        if month is None:
            month = datetime.now().strftime('%Y-%m')
        
        cursor = self.conn.execute("""
            SELECT category, SUM(amount) as total
            FROM expenses
            WHERE strftime('%Y-%m', date) = ?
            GROUP BY category
            ORDER BY total DESC
        """, (month,))
        
        return cursor.fetchall()
    
    def set_budget(self, category, limit):
        self.conn.execute("""
            INSERT OR REPLACE INTO budgets (category, monthly_limit)
            VALUES (?, ?)
        """, (category.lower(), limit))
        self.conn.commit()
        print(f"✅ Budget set: ₹{limit} for {category}")
    
    def check_budget(self, category):
        month = datetime.now().strftime('%Y-%m')
        
        cursor = self.conn.execute("""
            SELECT b.monthly_limit, COALESCE(SUM(e.amount), 0) as spent
            FROM budgets b
            LEFT JOIN expenses e ON e.category = b.category
                AND strftime('%Y-%m', e.date) = ?
            WHERE b.category = ?
        """, (month, category.lower()))
        
        result = cursor.fetchone()
        if result and result[0]:
            limit, spent = result
            if spent > limit * 0.8:
                remaining = limit - spent
                print(f"⚠️ Warning: {category} budget almost exhausted! Remaining: ₹{remaining:.2f}")
    
    def export_csv(self, filename="expenses_export.csv"):
        expenses = self.get_expenses(days=365)
        
        with open(filename, 'w', newline='') as f:
            writer = csv.writer(f)
            writer.writerow(['ID', 'Amount', 'Category', 'Description', 'Date'])
            writer.writerows(expenses)
        
        print(f"✅ Exported to {filename}")
    
    def display_summary(self):
        summary = self.get_summary()
        total = sum(amount for _, amount in summary)
        
        print("\n" + "=" * 40)
        print("📊 MONTHLY EXPENSE SUMMARY")
        print("=" * 40)
        
        for category, amount in summary:
            percentage = (amount / total * 100) if total > 0 else 0
            bar = "█" * int(percentage / 5)
            print(f"{category:15} ₹{amount:>10.2f} {bar} {percentage:.1f}%")
        
        print("-" * 40)
        print(f"{'TOTAL':15} ₹{total:>10.2f}")
        print("=" * 40)

def main():
    parser = argparse.ArgumentParser(description="Personal Expense Tracker")
    subparsers = parser.add_subparsers(dest="command")
    
    # Add expense
    add_parser = subparsers.add_parser("add", help="Add an expense")
    add_parser.add_argument("amount", type=float, help="Expense amount")
    add_parser.add_argument("category", help="Expense category")
    add_parser.add_argument("-d", "--description", default="", help="Description")
    
    # List expenses
    list_parser = subparsers.add_parser("list", help="List expenses")
    list_parser.add_argument("-n", "--days", type=int, default=30, help="Number of days")
    list_parser.add_argument("-c", "--category", help="Filter by category")
    
    # Summary
    subparsers.add_parser("summary", help="Show monthly summary")
    
    # Set budget
    budget_parser = subparsers.add_parser("budget", help="Set category budget")
    budget_parser.add_argument("category", help="Category name")
    budget_parser.add_argument("limit", type=float, help="Monthly limit")
    
    # Export
    subparsers.add_parser("export", help="Export to CSV")
    
    args = parser.parse_args()
    tracker = ExpenseTracker()
    
    if args.command == "add":
        tracker.add_expense(args.amount, args.category, args.description)
    elif args.command == "list":
        expenses = tracker.get_expenses(args.days, args.category)
        for exp in expenses:
            print(f"{exp[4]} | ₹{exp[1]:.2f} | {exp[2]} | {exp[3]}")
    elif args.command == "summary":
        tracker.display_summary()
    elif args.command == "budget":
        tracker.set_budget(args.category, args.limit)
    elif args.command == "export":
        tracker.export_csv()
    else:
        parser.print_help()

if __name__ == "__main__":
    main()
```

**Usage:**
```bash
python expense_tracker.py add 500 food -d "Dinner at restaurant"
python expense_tracker.py add 2000 transport -d "Monthly metro pass"
python expense_tracker.py list --days 7
python expense_tracker.py summary
python expense_tracker.py budget food 5000
python expense_tracker.py export
```

---

### Project 2: Password Manager

**Problem Statement:** Secure password manager with encryption.

**Features:**
- Master password authentication
- Add/View/Delete passwords
- Password generation
- AES encryption
- Search functionality

**Tech Stack:** Python, SQLite, cryptography

```python
# password_manager.py
import sqlite3
import secrets
import string
import hashlib
import base64
from getpass import getpass
from cryptography.fernet import Fernet
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC

class PasswordManager:
    def __init__(self):
        self.conn = sqlite3.connect("passwords.db")
        self.fernet = None
        self.setup_database()
    
    def setup_database(self):
        self.conn.execute("""
            CREATE TABLE IF NOT EXISTS master (
                id INTEGER PRIMARY KEY,
                password_hash TEXT NOT NULL,
                salt TEXT NOT NULL
            )
        """)
        
        self.conn.execute("""
            CREATE TABLE IF NOT EXISTS passwords (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                site TEXT NOT NULL,
                username TEXT NOT NULL,
                password TEXT NOT NULL,
                notes TEXT,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
            )
        """)
        self.conn.commit()
    
    def _derive_key(self, password: str, salt: bytes) -> bytes:
        """Derive encryption key from master password"""
        kdf = PBKDF2HMAC(
            algorithm=hashes.SHA256(),
            length=32,
            salt=salt,
            iterations=480000,
        )
        key = base64.urlsafe_b64encode(kdf.derive(password.encode()))
        return key
    
    def _hash_password(self, password: str, salt: bytes) -> str:
        """Hash password for verification"""
        return hashlib.pbkdf2_hmac(
            'sha256', password.encode(), salt, 100000
        ).hex()
    
    def is_initialized(self) -> bool:
        """Check if master password is set"""
        cursor = self.conn.execute("SELECT COUNT(*) FROM master")
        return cursor.fetchone()[0] > 0
    
    def initialize(self, master_password: str):
        """Set up master password"""
        salt = secrets.token_bytes(32)
        password_hash = self._hash_password(master_password, salt)
        
        self.conn.execute(
            "INSERT INTO master (password_hash, salt) VALUES (?, ?)",
            (password_hash, base64.b64encode(salt).decode())
        )
        self.conn.commit()
        
        self.fernet = Fernet(self._derive_key(master_password, salt))
        print("✅ Master password set successfully!")
    
    def authenticate(self, master_password: str) -> bool:
        """Verify master password"""
        cursor = self.conn.execute(
            "SELECT password_hash, salt FROM master LIMIT 1"
        )
        result = cursor.fetchone()
        
        if not result:
            return False
        
        stored_hash, salt_b64 = result
        salt = base64.b64decode(salt_b64)
        
        if self._hash_password(master_password, salt) == stored_hash:
            self.fernet = Fernet(self._derive_key(master_password, salt))
            return True
        return False
    
    def add_password(self, site: str, username: str, password: str, notes: str = ""):
        """Add encrypted password"""
        encrypted = self.fernet.encrypt(password.encode()).decode()
        
        self.conn.execute(
            "INSERT INTO passwords (site, username, password, notes) VALUES (?, ?, ?, ?)",
            (site, username, encrypted, notes)
        )
        self.conn.commit()
        print(f"✅ Password saved for {site}")
    
    def get_password(self, site: str) -> list:
        """Get passwords for site"""
        cursor = self.conn.execute(
            "SELECT site, username, password, notes FROM passwords WHERE site LIKE ?",
            (f"%{site}%",)
        )
        
        results = []
        for row in cursor.fetchall():
            decrypted = self.fernet.decrypt(row[2].encode()).decode()
            results.append({
                "site": row[0],
                "username": row[1],
                "password": decrypted,
                "notes": row[3]
            })
        return results
    
    def list_sites(self) -> list:
        """List all saved sites"""
        cursor = self.conn.execute(
            "SELECT DISTINCT site FROM passwords ORDER BY site"
        )
        return [row[0] for row in cursor.fetchall()]
    
    def delete_password(self, site: str):
        """Delete password entry"""
        self.conn.execute("DELETE FROM passwords WHERE site = ?", (site,))
        self.conn.commit()
        print(f"✅ Deleted password for {site}")
    
    @staticmethod
    def generate_password(length: int = 16, include_special: bool = True) -> str:
        """Generate secure random password"""
        chars = string.ascii_letters + string.digits
        if include_special:
            chars += "!@#$%^&*"
        
        # Ensure at least one of each required type
        password = [
            secrets.choice(string.ascii_lowercase),
            secrets.choice(string.ascii_uppercase),
            secrets.choice(string.digits),
        ]
        if include_special:
            password.append(secrets.choice("!@#$%^&*"))
        
        # Fill rest randomly
        password.extend(secrets.choice(chars) for _ in range(length - len(password)))
        
        # Shuffle
        secrets.SystemRandom().shuffle(password)
        return ''.join(password)

def main():
    pm = PasswordManager()
    
    if not pm.is_initialized():
        print("🔐 First time setup - Create master password")
        master = getpass("Enter master password: ")
        confirm = getpass("Confirm master password: ")
        
        if master != confirm:
            print("❌ Passwords don't match!")
            return
        
        pm.initialize(master)
    else:
        master = getpass("Enter master password: ")
        if not pm.authenticate(master):
            print("❌ Invalid password!")
            return
        print("✅ Authenticated!")
    
    while True:
        print("\n" + "=" * 40)
        print("🔐 PASSWORD MANAGER")
        print("=" * 40)
        print("1. Add password")
        print("2. Get password")
        print("3. Generate password")
        print("4. List sites")
        print("5. Delete password")
        print("6. Exit")
        
        choice = input("\nChoice: ")
        
        if choice == "1":
            site = input("Site: ")
            username = input("Username: ")
            password = getpass("Password (empty to generate): ")
            if not password:
                password = pm.generate_password()
                print(f"Generated: {password}")
            notes = input("Notes (optional): ")
            pm.add_password(site, username, password, notes)
        
        elif choice == "2":
            site = input("Search site: ")
            results = pm.get_password(site)
            for r in results:
                print(f"\nSite: {r['site']}")
                print(f"Username: {r['username']}")
                print(f"Password: {r['password']}")
                if r['notes']:
                    print(f"Notes: {r['notes']}")
        
        elif choice == "3":
            length = int(input("Length (default 16): ") or 16)
            print(f"Generated: {pm.generate_password(length)}")
        
        elif choice == "4":
            sites = pm.list_sites()
            print("\nSaved sites:")
            for site in sites:
                print(f"  - {site}")
        
        elif choice == "5":
            site = input("Site to delete: ")
            pm.delete_password(site)
        
        elif choice == "6":
            print("Goodbye! 👋")
            break

if __name__ == "__main__":
    main()
```

---

## 🟡 INTERMEDIATE PROJECTS

### Project 3: URL Shortener API

**Problem Statement:** Build a URL shortener service with analytics.

**Features:**
- Shorten long URLs
- Custom short codes
- Click analytics
- Expiration dates
- REST API

**Tech Stack:** FastAPI, SQLite, Redis (optional)

```python
# url_shortener.py
from fastapi import FastAPI, HTTPException, Request, Depends
from fastapi.responses import RedirectResponse
from pydantic import BaseModel, HttpUrl
from typing import Optional
import sqlite3
import hashlib
import string
import random
from datetime import datetime, timedelta
from contextlib import contextmanager

app = FastAPI(title="URL Shortener API")

DATABASE = "urls.db"

# Database setup
def init_db():
    with get_db() as conn:
        conn.execute("""
            CREATE TABLE IF NOT EXISTS urls (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                short_code TEXT UNIQUE NOT NULL,
                original_url TEXT NOT NULL,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                expires_at TIMESTAMP,
                clicks INTEGER DEFAULT 0
            )
        """)
        
        conn.execute("""
            CREATE TABLE IF NOT EXISTS clicks (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                url_id INTEGER NOT NULL,
                clicked_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                ip_address TEXT,
                user_agent TEXT,
                referer TEXT,
                FOREIGN KEY (url_id) REFERENCES urls (id)
            )
        """)

@contextmanager
def get_db():
    conn = sqlite3.connect(DATABASE)
    conn.row_factory = sqlite3.Row
    try:
        yield conn
        conn.commit()
    except Exception:
        conn.rollback()
        raise
    finally:
        conn.close()

# Pydantic models
class URLCreate(BaseModel):
    url: HttpUrl
    custom_code: Optional[str] = None
    expires_in_days: Optional[int] = None

class URLResponse(BaseModel):
    short_code: str
    short_url: str
    original_url: str
    created_at: str
    expires_at: Optional[str]

class URLStats(BaseModel):
    short_code: str
    original_url: str
    total_clicks: int
    created_at: str
    recent_clicks: list

# Helper functions
def generate_short_code(length=6):
    chars = string.ascii_letters + string.digits
    return ''.join(random.choices(chars, k=length))

def get_url_by_code(short_code: str):
    with get_db() as conn:
        cursor = conn.execute(
            "SELECT * FROM urls WHERE short_code = ?",
            (short_code,)
        )
        return cursor.fetchone()

# API Routes
@app.on_event("startup")
async def startup():
    init_db()

@app.post("/shorten", response_model=URLResponse)
async def create_short_url(url_data: URLCreate, request: Request):
    """Create a shortened URL"""
    
    # Use custom code or generate one
    if url_data.custom_code:
        short_code = url_data.custom_code
        # Check if custom code already exists
        if get_url_by_code(short_code):
            raise HTTPException(400, "Custom code already in use")
    else:
        # Generate unique code
        short_code = generate_short_code()
        while get_url_by_code(short_code):
            short_code = generate_short_code()
    
    # Calculate expiration
    expires_at = None
    if url_data.expires_in_days:
        expires_at = datetime.now() + timedelta(days=url_data.expires_in_days)
    
    with get_db() as conn:
        conn.execute(
            "INSERT INTO urls (short_code, original_url, expires_at) VALUES (?, ?, ?)",
            (short_code, str(url_data.url), expires_at)
        )
    
    base_url = str(request.base_url)
    
    return URLResponse(
        short_code=short_code,
        short_url=f"{base_url}{short_code}",
        original_url=str(url_data.url),
        created_at=datetime.now().isoformat(),
        expires_at=expires_at.isoformat() if expires_at else None
    )

@app.get("/{short_code}")
async def redirect_url(short_code: str, request: Request):
    """Redirect to original URL"""
    
    url = get_url_by_code(short_code)
    
    if not url:
        raise HTTPException(404, "URL not found")
    
    # Check expiration
    if url["expires_at"]:
        expires = datetime.fromisoformat(url["expires_at"])
        if datetime.now() > expires:
            raise HTTPException(410, "URL has expired")
    
    # Record click
    with get_db() as conn:
        conn.execute(
            "UPDATE urls SET clicks = clicks + 1 WHERE id = ?",
            (url["id"],)
        )
        
        conn.execute("""
            INSERT INTO clicks (url_id, ip_address, user_agent, referer)
            VALUES (?, ?, ?, ?)
        """, (
            url["id"],
            request.client.host,
            request.headers.get("user-agent"),
            request.headers.get("referer")
        ))
    
    return RedirectResponse(url["original_url"])

@app.get("/stats/{short_code}", response_model=URLStats)
async def get_url_stats(short_code: str):
    """Get analytics for a shortened URL"""
    
    url = get_url_by_code(short_code)
    
    if not url:
        raise HTTPException(404, "URL not found")
    
    with get_db() as conn:
        cursor = conn.execute("""
            SELECT clicked_at, ip_address, user_agent, referer
            FROM clicks
            WHERE url_id = ?
            ORDER BY clicked_at DESC
            LIMIT 10
        """, (url["id"],))
        
        recent_clicks = [
            {
                "clicked_at": row["clicked_at"],
                "ip_address": row["ip_address"][:10] + "***",
                "user_agent": row["user_agent"][:50] if row["user_agent"] else None
            }
            for row in cursor.fetchall()
        ]
    
    return URLStats(
        short_code=short_code,
        original_url=url["original_url"],
        total_clicks=url["clicks"],
        created_at=url["created_at"],
        recent_clicks=recent_clicks
    )

@app.delete("/{short_code}")
async def delete_url(short_code: str):
    """Delete a shortened URL"""
    
    url = get_url_by_code(short_code)
    
    if not url:
        raise HTTPException(404, "URL not found")
    
    with get_db() as conn:
        conn.execute("DELETE FROM clicks WHERE url_id = ?", (url["id"],))
        conn.execute("DELETE FROM urls WHERE id = ?", (url["id"],))
    
    return {"message": "URL deleted successfully"}

# Run: uvicorn url_shortener:app --reload
```

---

### Project 4: Task Queue System

**Problem Statement:** Distributed task queue for background job processing.

**Tech Stack:** Python, Redis, Threading/Multiprocessing

```python
# task_queue.py
import redis
import json
import uuid
import time
import threading
from typing import Callable, Any, Dict
from dataclasses import dataclass, asdict
from enum import Enum
from datetime import datetime
import traceback

class TaskStatus(Enum):
    PENDING = "pending"
    RUNNING = "running"
    COMPLETED = "completed"
    FAILED = "failed"

@dataclass
class Task:
    id: str
    name: str
    args: tuple
    kwargs: dict
    status: str
    created_at: str
    started_at: str = None
    completed_at: str = None
    result: Any = None
    error: str = None

class TaskQueue:
    """Redis-based task queue"""
    
    def __init__(self, redis_url="redis://localhost:6379", queue_name="tasks"):
        self.redis = redis.from_url(redis_url, decode_responses=True)
        self.queue_name = queue_name
        self.task_registry: Dict[str, Callable] = {}
    
    def task(self, func: Callable) -> Callable:
        """Decorator to register a task"""
        self.task_registry[func.__name__] = func
        
        def wrapper(*args, **kwargs):
            return self.enqueue(func.__name__, *args, **kwargs)
        
        wrapper.delay = lambda *args, **kwargs: self.enqueue(
            func.__name__, *args, **kwargs
        )
        return wrapper
    
    def enqueue(self, task_name: str, *args, **kwargs) -> str:
        """Add task to queue"""
        task_id = str(uuid.uuid4())
        
        task = Task(
            id=task_id,
            name=task_name,
            args=args,
            kwargs=kwargs,
            status=TaskStatus.PENDING.value,
            created_at=datetime.now().isoformat()
        )
        
        # Store task details
        self.redis.hset(f"task:{task_id}", mapping={
            k: json.dumps(v) if not isinstance(v, str) else v
            for k, v in asdict(task).items()
        })
        
        # Add to queue
        self.redis.rpush(self.queue_name, task_id)
        
        print(f"📥 Enqueued task: {task_name} ({task_id})")
        return task_id
    
    def get_task(self, task_id: str) -> Task:
        """Get task status"""
        data = self.redis.hgetall(f"task:{task_id}")
        if not data:
            return None
        
        return Task(
            id=data["id"],
            name=data["name"],
            args=json.loads(data["args"]),
            kwargs=json.loads(data["kwargs"]),
            status=data["status"],
            created_at=data["created_at"],
            started_at=data.get("started_at"),
            completed_at=data.get("completed_at"),
            result=json.loads(data["result"]) if data.get("result") else None,
            error=data.get("error")
        )
    
    def update_task(self, task_id: str, **updates):
        """Update task fields"""
        for key, value in updates.items():
            if not isinstance(value, str):
                value = json.dumps(value)
            self.redis.hset(f"task:{task_id}", key, value)

class Worker:
    """Task queue worker"""
    
    def __init__(self, queue: TaskQueue, worker_id: str = None):
        self.queue = queue
        self.worker_id = worker_id or str(uuid.uuid4())[:8]
        self.running = False
    
    def process_task(self, task_id: str):
        """Process a single task"""
        task = self.queue.get_task(task_id)
        
        if not task:
            print(f"⚠️ Task {task_id} not found")
            return
        
        if task.name not in self.queue.task_registry:
            self.queue.update_task(task_id, 
                status=TaskStatus.FAILED.value,
                error=f"Unknown task: {task.name}"
            )
            return
        
        func = self.queue.task_registry[task.name]
        
        # Update status to running
        self.queue.update_task(task_id,
            status=TaskStatus.RUNNING.value,
            started_at=datetime.now().isoformat()
        )
        
        print(f"🔄 Worker {self.worker_id} processing: {task.name} ({task_id})")
        
        try:
            result = func(*task.args, **task.kwargs)
            
            self.queue.update_task(task_id,
                status=TaskStatus.COMPLETED.value,
                completed_at=datetime.now().isoformat(),
                result=result
            )
            print(f"✅ Task completed: {task.name} ({task_id})")
            
        except Exception as e:
            self.queue.update_task(task_id,
                status=TaskStatus.FAILED.value,
                completed_at=datetime.now().isoformat(),
                error=str(e)
            )
            print(f"❌ Task failed: {task.name} ({task_id}) - {e}")
    
    def run(self):
        """Start worker loop"""
        self.running = True
        print(f"🚀 Worker {self.worker_id} started")
        
        while self.running:
            # Blocking pop from queue
            result = self.queue.redis.blpop(self.queue.queue_name, timeout=1)
            
            if result:
                _, task_id = result
                self.process_task(task_id)
    
    def stop(self):
        """Stop worker"""
        self.running = False
        print(f"🛑 Worker {self.worker_id} stopped")

# Usage Example
if __name__ == "__main__":
    # Initialize queue
    queue = TaskQueue()
    
    # Register tasks
    @queue.task
    def send_email(to: str, subject: str, body: str):
        """Simulate sending email"""
        print(f"Sending email to {to}...")
        time.sleep(2)  # Simulate work
        return {"status": "sent", "to": to}
    
    @queue.task
    def process_image(image_path: str, operations: list):
        """Simulate image processing"""
        print(f"Processing image: {image_path}")
        time.sleep(3)  # Simulate work
        return {"processed": image_path, "operations": operations}
    
    # Start worker in background thread
    worker = Worker(queue)
    worker_thread = threading.Thread(target=worker.run, daemon=True)
    worker_thread.start()
    
    # Enqueue tasks
    task1_id = send_email.delay("user@example.com", "Hello", "Test email")
    task2_id = process_image.delay("/path/to/image.jpg", ["resize", "filter"])
    
    # Wait and check results
    time.sleep(6)
    
    print("\n📊 Task Results:")
    print(f"Task 1: {queue.get_task(task1_id)}")
    print(f"Task 2: {queue.get_task(task2_id)}")
    
    worker.stop()
```

---

## 🔴 ADVANCED PROJECT

### Project 5: Real-time Chat Application

**Problem Statement:** Full-stack real-time chat with WebSocket.

**Features:**
- User authentication
- Real-time messaging
- Chat rooms
- Message history
- Online status
- File sharing

**Tech Stack:** FastAPI, WebSocket, SQLAlchemy, Redis, HTML/JS

```python
# chat_server.py
from fastapi import FastAPI, WebSocket, WebSocketDisconnect, HTTPException, Depends
from fastapi.staticfiles import StaticFiles
from fastapi.responses import HTMLResponse
from pydantic import BaseModel
from typing import Dict, List, Set
import json
import asyncio
from datetime import datetime
import uuid

app = FastAPI(title="Real-time Chat")

# Connection Manager
class ConnectionManager:
    def __init__(self):
        # room_id -> set of websockets
        self.active_connections: Dict[str, Set[WebSocket]] = {}
        # websocket -> user info
        self.user_info: Dict[WebSocket, dict] = {}
    
    async def connect(self, websocket: WebSocket, room_id: str, user: dict):
        await websocket.accept()
        
        if room_id not in self.active_connections:
            self.active_connections[room_id] = set()
        
        self.active_connections[room_id].add(websocket)
        self.user_info[websocket] = {**user, "room_id": room_id}
        
        # Notify others
        await self.broadcast(room_id, {
            "type": "user_joined",
            "user": user["username"],
            "timestamp": datetime.now().isoformat()
        }, exclude=websocket)
    
    def disconnect(self, websocket: WebSocket):
        user = self.user_info.get(websocket)
        if user:
            room_id = user["room_id"]
            self.active_connections[room_id].discard(websocket)
            del self.user_info[websocket]
            return user
        return None
    
    async def broadcast(self, room_id: str, message: dict, exclude: WebSocket = None):
        if room_id in self.active_connections:
            for connection in self.active_connections[room_id]:
                if connection != exclude:
                    try:
                        await connection.send_json(message)
                    except:
                        pass
    
    def get_online_users(self, room_id: str) -> List[str]:
        if room_id not in self.active_connections:
            return []
        return [
            self.user_info[ws]["username"]
            for ws in self.active_connections[room_id]
            if ws in self.user_info
        ]

manager = ConnectionManager()

# Message storage (in-memory, use database in production)
messages_store: Dict[str, List[dict]] = {}

# Models
class Message(BaseModel):
    content: str
    message_type: str = "text"

# WebSocket endpoint
@app.websocket("/ws/{room_id}/{username}")
async def websocket_endpoint(websocket: WebSocket, room_id: str, username: str):
    user = {"username": username, "user_id": str(uuid.uuid4())}
    
    await manager.connect(websocket, room_id, user)
    
    try:
        # Send message history
        if room_id in messages_store:
            await websocket.send_json({
                "type": "history",
                "messages": messages_store[room_id][-50:]  # Last 50 messages
            })
        
        # Send online users
        await websocket.send_json({
            "type": "online_users",
            "users": manager.get_online_users(room_id)
        })
        
        while True:
            data = await websocket.receive_json()
            
            if data["type"] == "message":
                message = {
                    "id": str(uuid.uuid4()),
                    "type": "message",
                    "content": data["content"],
                    "username": username,
                    "timestamp": datetime.now().isoformat()
                }
                
                # Store message
                if room_id not in messages_store:
                    messages_store[room_id] = []
                messages_store[room_id].append(message)
                
                # Broadcast to all in room
                await manager.broadcast(room_id, message)
            
            elif data["type"] == "typing":
                await manager.broadcast(room_id, {
                    "type": "typing",
                    "username": username
                }, exclude=websocket)
    
    except WebSocketDisconnect:
        user = manager.disconnect(websocket)
        if user:
            await manager.broadcast(user["room_id"], {
                "type": "user_left",
                "user": user["username"],
                "timestamp": datetime.now().isoformat()
            })

# REST endpoints
@app.get("/rooms/{room_id}/users")
async def get_room_users(room_id: str):
    return {"users": manager.get_online_users(room_id)}

@app.get("/rooms/{room_id}/messages")
async def get_room_messages(room_id: str, limit: int = 50):
    messages = messages_store.get(room_id, [])
    return {"messages": messages[-limit:]}

# HTML Client
HTML_CLIENT = """
<!DOCTYPE html>
<html>
<head>
    <title>Chat Room</title>
    <style>
        body { font-family: Arial; margin: 20px; }
        #chat { border: 1px solid #ccc; height: 400px; overflow-y: scroll; padding: 10px; margin-bottom: 10px; }
        .message { margin: 5px 0; padding: 5px; background: #f0f0f0; border-radius: 5px; }
        .system { color: #666; font-style: italic; }
        #input { width: 80%; padding: 10px; }
        #send { padding: 10px 20px; }
        #users { margin-top: 10px; }
    </style>
</head>
<body>
    <h1>Chat Room: <span id="room"></span></h1>
    <div id="chat"></div>
    <input type="text" id="input" placeholder="Type a message...">
    <button id="send">Send</button>
    <div id="users"><strong>Online:</strong> <span id="userList"></span></div>
    
    <script>
        const room = prompt("Enter room name:", "general");
        const username = prompt("Enter your username:");
        
        document.getElementById("room").textContent = room;
        
        const ws = new WebSocket(`ws://${location.host}/ws/${room}/${username}`);
        const chat = document.getElementById("chat");
        const input = document.getElementById("input");
        
        ws.onmessage = function(event) {
            const data = JSON.parse(event.data);
            
            if (data.type === "message") {
                addMessage(`${data.username}: ${data.content}`);
            } else if (data.type === "user_joined") {
                addMessage(`${data.user} joined the chat`, true);
            } else if (data.type === "user_left") {
                addMessage(`${data.user} left the chat`, true);
            } else if (data.type === "history") {
                data.messages.forEach(msg => {
                    addMessage(`${msg.username}: ${msg.content}`);
                });
            } else if (data.type === "online_users") {
                document.getElementById("userList").textContent = data.users.join(", ");
            }
        };
        
        function addMessage(text, isSystem = false) {
            const div = document.createElement("div");
            div.className = "message" + (isSystem ? " system" : "");
            div.textContent = text;
            chat.appendChild(div);
            chat.scrollTop = chat.scrollHeight;
        }
        
        function sendMessage() {
            if (input.value) {
                ws.send(JSON.stringify({type: "message", content: input.value}));
                input.value = "";
            }
        }
        
        document.getElementById("send").onclick = sendMessage;
        input.onkeypress = function(e) {
            if (e.key === "Enter") sendMessage();
        };
    </script>
</body>
</html>
"""

@app.get("/")
async def get_chat_client():
    return HTMLResponse(HTML_CLIENT)

# Run: uvicorn chat_server:app --reload
```

---

## 💼 INTERVIEW PREPARATION

## 📝 Top 50 Python Interview Questions

### Basic Level (1-15)

**1. Python kya hai? Features batao.**
```
- High-level, interpreted language
- Dynamically typed
- Object-oriented and functional
- Extensive standard library
- Cross-platform
```

**2. List vs Tuple vs Set vs Dictionary**
```python
list_ex = [1, 2, 3]      # Ordered, mutable, duplicates allowed
tuple_ex = (1, 2, 3)     # Ordered, immutable, duplicates allowed
set_ex = {1, 2, 3}       # Unordered, mutable, no duplicates
dict_ex = {"a": 1}       # Key-value pairs, keys unique
```

**3. `is` vs `==` difference**
```python
a = [1, 2, 3]
b = [1, 2, 3]
print(a == b)  # True (values equal)
print(a is b)  # False (different objects)
```

**4. Shallow copy vs Deep copy**
```python
import copy
original = [[1, 2], [3, 4]]
shallow = copy.copy(original)    # Nested objects shared
deep = copy.deepcopy(original)   # Completely independent
```

**5. `*args` and `**kwargs`**
```python
def func(*args, **kwargs):
    print(args)    # Tuple of positional args
    print(kwargs)  # Dict of keyword args

func(1, 2, 3, name="Rahul", age=25)
# (1, 2, 3)
# {'name': 'Rahul', 'age': 25}
```

**6. Lambda function kya hai?**
```python
# Anonymous, single-expression function
square = lambda x: x ** 2
print(square(5))  # 25

# Common use with map, filter
nums = [1, 2, 3, 4, 5]
evens = list(filter(lambda x: x % 2 == 0, nums))
```

**7. List comprehension explain karo**
```python
# [expression for item in iterable if condition]
squares = [x**2 for x in range(10) if x % 2 == 0]
# [0, 4, 16, 36, 64]
```

**8. Generator kya hai?**
```python
# Memory-efficient iterator using yield
def countdown(n):
    while n > 0:
        yield n
        n -= 1

for num in countdown(5):
    print(num)
```

**9. Decorator explain karo**
```python
def timer(func):
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        print(f"Time: {time.time() - start:.2f}s")
        return result
    return wrapper

@timer
def slow_function():
    import time
    time.sleep(1)
```

**10. `__init__` vs `__new__`**
```python
class MyClass:
    def __new__(cls, *args):
        # Creates instance
        instance = super().__new__(cls)
        return instance
    
    def __init__(self, value):
        # Initializes instance
        self.value = value
```

### Intermediate Level (16-35)

**11. GIL (Global Interpreter Lock) kya hai?**
```
- CPython mein only one thread executes Python bytecode at a time
- CPU-bound tasks ke liye multiprocessing use karo
- I/O-bound tasks ke liye threading ya asyncio use karo
```

**12. Multithreading vs Multiprocessing**
```python
# Threading - I/O bound tasks
import threading
thread = threading.Thread(target=func)

# Multiprocessing - CPU bound tasks
import multiprocessing
process = multiprocessing.Process(target=func)
```

**13. Context manager explain karo**
```python
class FileManager:
    def __init__(self, filename):
        self.filename = filename
    
    def __enter__(self):
        self.file = open(self.filename, 'r')
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()
        return False  # Don't suppress exceptions
```

**14. Memory management in Python**
```
- Reference counting
- Garbage collection (cyclic references)
- Memory pool for small objects
- PyMalloc allocator
```

**15. Method Resolution Order (MRO)**
```python
class A: pass
class B(A): pass
class C(A): pass
class D(B, C): pass

print(D.__mro__)
# (D, B, C, A, object) - C3 linearization
```

**16. `classmethod` vs `staticmethod`**
```python
class MyClass:
    class_var = "shared"
    
    @classmethod
    def class_method(cls):
        return cls.class_var
    
    @staticmethod
    def static_method():
        return "No access to cls or self"
```

**17. Metaclass kya hai?**
```python
class Meta(type):
    def __new__(mcs, name, bases, namespace):
        # Customize class creation
        return super().__new__(mcs, name, bases, namespace)

class MyClass(metaclass=Meta):
    pass
```

**18. Descriptors explain karo**
```python
class Validator:
    def __get__(self, obj, type=None):
        return obj.__dict__.get(self.name)
    
    def __set__(self, obj, value):
        if value < 0:
            raise ValueError("Must be positive")
        obj.__dict__[self.name] = value
    
    def __set_name__(self, owner, name):
        self.name = name
```

**19. `property` decorator**
```python
class Circle:
    def __init__(self, radius):
        self._radius = radius
    
    @property
    def radius(self):
        return self._radius
    
    @radius.setter
    def radius(self, value):
        if value <= 0:
            raise ValueError("Radius must be positive")
        self._radius = value
```

**20. Asyncio basics**
```python
import asyncio

async def fetch_data():
    await asyncio.sleep(1)
    return "Data"

async def main():
    result = await fetch_data()
    print(result)

asyncio.run(main())
```

### Advanced Level (36-50)

**21. Memory leak kaise detect karein?**
```python
import tracemalloc

tracemalloc.start()
# ... your code ...
snapshot = tracemalloc.take_snapshot()
top_stats = snapshot.statistics('lineno')
for stat in top_stats[:10]:
    print(stat)
```

**22. `__slots__` ka use**
```python
class Optimized:
    __slots__ = ['x', 'y']  # No __dict__, saves memory
    
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

**23. Weak references**
```python
import weakref

class MyClass:
    pass

obj = MyClass()
weak_ref = weakref.ref(obj)
print(weak_ref())  # Object
del obj
print(weak_ref())  # None (garbage collected)
```

**24. Python internals - Bytecode**
```python
import dis

def func(x, y):
    return x + y

dis.dis(func)
# Shows bytecode instructions
```

**25. Monkey patching**
```python
import some_module

# Replace function at runtime
original_func = some_module.func
def patched_func():
    print("Patched!")
    return original_func()

some_module.func = patched_func
```

---

## 🗺️ PYTHON ROADMAP

### 📍 Beginner (0-3 months)
```
Week 1-2: Basics
├── Variables, Data Types
├── Operators
├── Input/Output
└── Strings

Week 3-4: Control Flow
├── Conditions (if/elif/else)
├── Loops (for/while)
└── List Comprehensions

Week 5-6: Functions
├── Function Definition
├── Parameters/Arguments
├── Lambda Functions
└── Scope

Week 7-8: Data Structures
├── Lists
├── Tuples
├── Sets
└── Dictionaries

Week 9-10: OOP Basics
├── Classes and Objects
├── Inheritance
├── Encapsulation
└── Polymorphism

Week 11-12: File Handling
├── Reading/Writing Files
├── CSV/JSON
└── Exception Handling
```

### 📍 Intermediate (3-6 months)
```
Month 4: Advanced Python
├── Decorators
├── Generators
├── Context Managers
└── Itertools

Month 5: Libraries
├── NumPy
├── Pandas
├── Matplotlib
└── Requests

Month 6: Databases & APIs
├── SQL (SQLite/PostgreSQL)
├── SQLAlchemy ORM
├── REST API (FastAPI)
└── Authentication
```

### 📍 Advanced (6-12 months)
```
Month 7-8: Specialization
├── Web Development (Django/FastAPI)
├── Data Science (ML basics)
├── Automation (Selenium)
└── DevOps (Docker, CI/CD)

Month 9-10: Projects
├── Full-stack Application
├── Data Pipeline
├── CLI Tool
└── API Service

Month 11-12: Interview Prep
├── DSA with Python
├── System Design
├── Mock Interviews
└── Portfolio Building
```

---

## 🔁 HOW TO REVISE EFFECTIVELY

### Daily Routine
```
Morning (30 min):
- Review yesterday's concepts
- Read documentation

Evening (1-2 hours):
- Code practice
- Build projects
- Solve problems
```

### Weekly Plan
```
Monday: New concepts
Tuesday: Practice problems
Wednesday: Project work
Thursday: Code review
Friday: Mock interview
Weekend: Build/contribute to projects
```

### Revision Techniques
```
1. Spaced Repetition
   - Day 1: Learn
   - Day 3: Review
   - Day 7: Review
   - Day 14: Review
   - Day 30: Review

2. Active Recall
   - Close book, try to explain
   - Write code without looking
   - Teach someone

3. Feynman Technique
   - Learn concept
   - Explain in simple terms
   - Identify gaps
   - Review and simplify
```

---

## 📚 BEST RESOURCES

### 📺 YouTube Channels
```
English:
- Corey Schafer
- Tech With Tim
- Sentdex
- ArjanCodes

Hindi:
- CodeWithHarry
- Telusko
- MySirG
```

### 📖 Documentation
```
- docs.python.org (Official)
- realpython.com
- w3schools.com/python
- geeksforgeeks.org
```

### 💻 Practice Platforms
```
Beginner:
- HackerRank
- Codecademy
- SoloLearn

Intermediate:
- LeetCode
- CodeSignal
- Exercism

Advanced:
- Codeforces
- TopCoder
- Project Euler
```

### 📚 Books
```
Beginner:
- "Automate the Boring Stuff" - Al Sweigart
- "Python Crash Course" - Eric Matthes

Intermediate:
- "Fluent Python" - Luciano Ramalho
- "Effective Python" - Brett Slatkin

Advanced:
- "Python Cookbook" - David Beazley
- "High Performance Python" - Gorelick & Ozsvald
```

### 🛠️ Tools
```
IDEs:
- VS Code + Python extension
- PyCharm
- Jupyter Notebook

Package Management:
- pip
- conda
- poetry

Version Control:
- Git/GitHub
```

---

## 🏆 Final Tips

### For Learning:
```
1. Code daily (even 30 minutes)
2. Build projects, not just tutorials
3. Read others' code
4. Contribute to open source
5. Join Python communities
```

### For Interviews:
```
1. Practice DSA problems daily
2. Understand time/space complexity
3. Know Python-specific features
4. Prepare system design basics
5. Have 2-3 solid projects
```

### For Career:
```
1. Choose a specialization
2. Build portfolio
3. Network actively
4. Keep learning
5. Share knowledge (blog, YouTube)
```

---

<div align="center">

## 🎉 Congratulations!

You've completed the **Python from Zero to Advanced** course!

Keep coding, keep learning, keep growing! 🚀

---

**Made with ❤️ for Python Learners**

© 2024 | Python Learning System

</div>
