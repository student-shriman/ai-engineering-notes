# Pydantic — Complete Reference Guide

---

## What is Pydantic?

**Pydantic** is a Python library for **data validation and settings management** using Python type annotations. It enforces type hints at runtime and provides user-friendly error messages when data is invalid.

Built on top of Python's standard type hints, Pydantic lets you define the shape of your data using simple Python classes — and it handles validation, parsing, and serialization automatically.

> Pydantic is the most widely used data validation library for Python, powering tools like **FastAPI**, **LangChain**, and many more.

---

## Purpose

- Validate incoming data (from APIs, forms, configs, databases)
- Ensure data types are correct before processing
- Provide clear, structured error messages
- Convert/parse raw data into Python objects automatically
- Serialize Python objects back into dicts or JSON

---

## Use Cases

| Use Case | Example |
|---|---|
| API request/response validation | FastAPI route bodies |
| Configuration management | App settings from env files |
| Data parsing from external sources | JSON from third-party APIs |
| Database schema validation | ORM model input sanitization |
| ML pipeline data contracts | Validating model inputs/outputs |
| CLI tool argument validation | Structured command-line configs |

---

## Applications

- **FastAPI** — uses Pydantic models for all request/response bodies
- **LangChain / LangGraph** — structured tool calls and agent outputs
- **Data pipelines** — validate data at ingestion boundaries
- **Microservices** — enforce contracts between services
- **Settings management** — `pydantic-settings` for `.env` files

---

## Installation

```bash
pip install pydantic
```

---

## Imports

```python
from pydantic import BaseModel, Field, field_validator, model_validator
from pydantic import ConfigDict
from typing import List, Dict, Any, Optional, Tuple
```

---

## 1. Defining a Basic Model

Every Pydantic model inherits from `BaseModel`. Fields are defined as class attributes with type annotations.

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
    email: str
```

```python
user = User(name="Alice", age=25, email="alice@example.com")
print(user)
# name='Alice' age=25 email='alice@example.com'
```

---

## 2. Automatic Type Casting

Pydantic **automatically converts** compatible types — you don't need to manually cast values. If you pass a string `"25"` where an `int` is expected, Pydantic will cast it silently.

```python
from pydantic import BaseModel

class Product(BaseModel):
    name: str
    price: float
    quantity: int

# Passing strings — Pydantic auto-casts them
product = Product(name="Laptop", price="999.99", quantity="5")

print(product.price)     # 999.99  (float, not string)
print(product.quantity)  # 5       (int, not string)
```

> If a value **cannot** be cast (e.g., `"abc"` → `int`), Pydantic raises a `ValidationError`.

---

## 3. Default Values

Fields can have **default values**. If a value is not provided during instantiation, the default is used.

```python
from pydantic import BaseModel

class Config(BaseModel):
    host: str = "localhost"
    port: int = 8080
    debug: bool = False

config = Config()
print(config.host)   # localhost
print(config.port)   # 8080

config2 = Config(host="0.0.0.0", port=443)
print(config2.port)  # 443
```

---

## 4. Optional Fields

Use `Optional[T]` from `typing` to mark a field as optional (can be `None`). Always pair with a default of `None`.

```python
from pydantic import BaseModel
from typing import Optional

class Employee(BaseModel):
    name: str
    department: str
    manager: Optional[str] = None      # This field is not required
    salary: Optional[float] = None
```

```python
emp = Employee(name="Bob", department="Engineering")
print(emp.manager)  # None
```

---

## 5. Using Typing — List, Dict, Any, Optional

Pydantic fully supports Python's `typing` module for complex types.

```python
from pydantic import BaseModel
from typing import List, Dict, Any, Optional

class Report(BaseModel):
    title: str
    tags: List[str]                        # List of strings
    metadata: Dict[str, Any]               # Dict with string keys, any values
    scores: List[float]
    owner: Optional[str] = None            # Optional string

report = Report(
    title="Q1 Analysis",
    tags=["finance", "quarterly"],
    metadata={"version": 1, "reviewed": True},
    scores=[95.5, 88.0, 76.3]
)

print(report.tags)      # ['finance', 'quarterly']
print(report.scores)    # [95.5, 88.0, 76.3]
```

---

## 6. Field — Constraints and Metadata

`Field()` is used to add extra constraints and metadata to individual fields.

### Common Field Parameters

| Parameter | Description |
|---|---|
| `default` | Default value for the field |
| `description` | Human-readable description (shown in API docs) |
| `examples` | List of example values |
| `min_length` | Minimum string/list length |
| `max_length` | Maximum string/list length |
| `gt` | Greater than (for numbers) |
| `ge` | Greater than or equal to |
| `lt` | Less than |
| `le` | Less than or equal to |
| `pattern` | Regex pattern for strings |

```python
from pydantic import BaseModel, Field
from typing import Optional

class UserProfile(BaseModel):
    username: str = Field(
        min_length=3,
        max_length=20,
        description="Unique username for the user",
        examples=["alice_23", "bob_dev"]
    )
    age: int = Field(
        ge=18,
        le=120,
        description="Age must be between 18 and 120"
    )
    bio: Optional[str] = Field(
        default=None,
        max_length=300,
        description="Short biography"
    )
    rating: float = Field(
        default=0.0,
        ge=0.0,
        le=5.0,
        description="Rating between 0.0 and 5.0"
    )
```

```python
# This will raise a ValidationError — username too short
user = UserProfile(username="ab", age=25)

# This works
user = UserProfile(username="alice_23", age=25, rating=4.5)
```

---

## 7. Nested Models

You can use one Pydantic model **as a field type** inside another — this creates nested/structured data.

```python
from pydantic import BaseModel
from typing import List

class Address(BaseModel):
    street: str
    city: str
    zip_code: str

class Order(BaseModel):
    item: str
    quantity: int
    price: float

class Customer(BaseModel):
    name: str
    address: Address          # Nested model
    orders: List[Order]       # List of nested models
```

```python
customer = Customer(
    name="Alice",
    address={"street": "123 Main St", "city": "New York", "zip_code": "10001"},
    orders=[
        {"item": "Laptop", "quantity": 1, "price": 999.99},
        {"item": "Mouse", "quantity": 2, "price": 29.99}
    ]
)

print(customer.address.city)       # New York
print(customer.orders[0].item)     # Laptop
```

---

## 8. Model Inheritance

One Pydantic model can **inherit** from another, extending its fields. Useful for sharing common fields across models.

```python
from pydantic import BaseModel, Field
from typing import Optional, List

# Base model with common fields
class BaseItem(BaseModel):
    id: int
    name: str
    created_at: Optional[str] = None

# Child model inheriting from BaseItem — adds extra fields
class DictItem(BaseItem):
    metadata: dict = {}

# Another child extending DictItem — adds a list of tags
class ListItem(DictItem):
    tags: List[str] = []
    description: Optional[str] = None
```

```python
item = ListItem(
    id=1,
    name="Sample",
    metadata={"source": "api"},
    tags=["python", "pydantic"]
)

print(item.id)        # 1  (inherited from BaseItem)
print(item.tags)      # ['python', 'pydantic']
print(item.metadata)  # {'source': 'api'}
```

---

## 9. Serialization

### `model_dump()` — Convert to Python Dictionary

```python
from pydantic import BaseModel

class Article(BaseModel):
    title: str
    author: str
    published: bool = True

article = Article(title="Pydantic Guide", author="Alice")

# Convert to dict
data = article.model_dump()
print(data)
# {'title': 'Pydantic Guide', 'author': 'Alice', 'published': True}

# Exclude certain fields
print(article.model_dump(exclude={"published"}))
# {'title': 'Pydantic Guide', 'author': 'Alice'}

# Include only specific fields
print(article.model_dump(include={"title"}))
# {'title': 'Pydantic Guide'}
```

---

### `model_dump_json()` — Convert to JSON String

```python
json_str = article.model_dump_json()
print(json_str)
# {"title":"Pydantic Guide","author":"Alice","published":true}

# Pretty print
import json
print(json.dumps(json.loads(article.model_dump_json()), indent=2))
```

---

### `model_validate()` — Parse from Dictionary

```python
data = {"title": "New Article", "author": "Bob"}
article = Article.model_validate(data)
print(article.title)  # New Article
```

---

## 10. ConfigDict — Model Configuration

`ConfigDict` lets you customize how your Pydantic model behaves.

```python
from pydantic import BaseModel, ConfigDict

class StrictUser(BaseModel):
    model_config = ConfigDict(
        str_strip_whitespace=True,   # Strip whitespace from strings
        str_to_lower=False,          # Don't lowercase strings
        frozen=False,                # Allow mutation after creation
        extra="forbid",              # Raise error if extra fields are passed
        populate_by_name=True,       # Allow populating by field name or alias
    )

    name: str
    email: str
```

```python
# Extra field 'role' will raise a ValidationError
user = StrictUser(name="  Alice  ", email="alice@example.com")
print(user.name)  # "Alice" (whitespace stripped)
```

### Common ConfigDict Options

| Option | Description |
|---|---|
| `str_strip_whitespace` | Strip leading/trailing whitespace from `str` fields |
| `frozen` | Make model immutable (like a frozen dataclass) |
| `extra="forbid"` | Raise error on unexpected extra fields |
| `extra="ignore"` | Silently ignore unexpected extra fields |
| `extra="allow"` | Accept and store unexpected extra fields |
| `populate_by_name` | Allow both field name and alias to be used |
| `validate_default` | Run validators even on default values |

---

## Full Example 1 — E-Commerce Order System

```python
from pydantic import BaseModel, Field, ConfigDict
from typing import List, Optional, Dict, Any

class Address(BaseModel):
    street: str
    city: str
    state: str
    zip_code: str = Field(min_length=5, max_length=10)
    country: str = "India"

class CartItem(BaseModel):
    product_id: str
    product_name: str
    quantity: int = Field(ge=1, le=100)
    unit_price: float = Field(gt=0)

    @property
    def total_price(self) -> float:
        return self.quantity * self.unit_price

class Order(BaseModel):
    model_config = ConfigDict(str_strip_whitespace=True)

    order_id: str
    customer_name: str = Field(min_length=2, max_length=100)
    shipping_address: Address
    items: List[CartItem]
    coupon_code: Optional[str] = None
    notes: Optional[str] = Field(default=None, max_length=500)
    metadata: Dict[str, Any] = {}

# Create an order
order = Order(
    order_id="ORD-001",
    customer_name="  Rahul Sharma  ",
    shipping_address={
        "street": "45 MG Road",
        "city": "Bangalore",
        "state": "Karnataka",
        "zip_code": "560001"
    },
    items=[
        {"product_id": "P01", "product_name": "Keyboard", "quantity": 2, "unit_price": 1500.0},
        {"product_id": "P02", "product_name": "Mouse", "quantity": "1", "unit_price": "799.50"}  # strings → auto-cast
    ],
    metadata={"source": "web", "priority": "high"}
)

print(order.customer_name)              # Rahul Sharma (whitespace stripped)
print(order.items[1].unit_price)        # 799.5 (auto-cast from string)
print(order.shipping_address.country)  # India (default value)
print(order.model_dump())              # Full dict
print(order.model_dump_json())         # JSON string
```

---

## Full Example 2 — User Registry with Inheritance

```python
from pydantic import BaseModel, Field, ConfigDict
from typing import Optional, List

# Base model
class BaseUser(BaseModel):
    id: int
    username: str = Field(min_length=3, max_length=30)
    email: str

# Extended model for registered users
class RegisteredUser(BaseUser):
    is_active: bool = True
    role: str = "viewer"

# Admin user inheriting from RegisteredUser
class AdminUser(RegisteredUser):
    model_config = ConfigDict(frozen=True)  # Immutable after creation

    role: str = "admin"
    permissions: List[str] = Field(
        default=["read", "write"],
        description="List of allowed permissions"
    )
    department: Optional[str] = None

# Usage
admin = AdminUser(
    id=1,
    username="shriman_admin",
    email="shriman@example.com",
    permissions=["read", "write", "delete"],
    department="Engineering"
)

print(admin.role)         # admin
print(admin.permissions)  # ['read', 'write', 'delete']

# Serialize
print(admin.model_dump())
# {'id': 1, 'username': 'shriman_admin', 'email': 'shriman@example.com',
#  'is_active': True, 'role': 'admin', 'permissions': [...], 'department': 'Engineering'}

print(admin.model_dump_json())
# JSON string equivalent

# Parse from dict
data = {
    "id": 2,
    "username": "dev_user",
    "email": "dev@example.com",
    "permissions": ["read"]
}
dev_admin = AdminUser.model_validate(data)
print(dev_admin.username)  # dev_user
```

---

## Quick Reference Cheat Sheet

```python
from pydantic import BaseModel, Field, ConfigDict
from typing import List, Dict, Any, Optional

class MyModel(BaseModel):
    model_config = ConfigDict(str_strip_whitespace=True, extra="forbid")

    # Basic field
    name: str

    # With default
    status: str = "active"

    # Optional
    nickname: Optional[str] = None

    # With constraints
    age: int = Field(ge=0, le=150, description="Age in years")

    # String constraints
    code: str = Field(min_length=3, max_length=10)

    # Complex types
    tags: List[str] = []
    config: Dict[str, Any] = {}

# Instantiate (auto type-cast happens here)
obj = MyModel(name="Alice", age="30", code="ABC")

# Serialize
obj.model_dump()                     # → dict
obj.model_dump_json()                # → JSON string
obj.model_dump(exclude={"status"})   # → dict without 'status'
obj.model_dump(include={"name"})     # → dict with only 'name'

# Parse
MyModel.model_validate({"name": "Bob", "age": 25, "code": "XYZ"})
```
