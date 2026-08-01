```markdown
# Data Types in Python & Security Considerations

> **"Data integrity begins with knowing your types."**  
> Understanding Python's dynamic typing, variable behaviors, and memory representations is key to building secure, bug-free applications.

---

## 📌 What are Data Types?

In Python, every value has a datatype, but variables do not need explicit declaration. Python is **dynamically typed**, meaning the type is determined automatically at runtime.

Checking a variable's data type:
```python
x = 42
print(type(x))  # <class 'int'>

```

---

## 🛠️ Primitive Data Types & Examples

### 1. Numeric Types

* **`int`**: Whole numbers (positive or negative) without decimals.
* **`float`**: Floating-point numbers representing real numbers with decimals.
* **`complex`**: Numbers with a real and imaginary part (e.g., `3 + 5j`).

```python
age = 25              # int
pi = 3.14159          # float
complex_num = 2 + 4j  # complex

```

### 2. Text / String (`str`)

Unchangeable (immutable) sequences of Unicode characters used for text manipulation.

```python
username = "admin_user"
greeting = 'Hello, World!'

```

### 3. Boolean (`bool`)

Represents truth values: either `True` or `False`. Used heavily in conditional evaluation and access control.

```python
is_authenticated = True
is_admin = False

```

---

## 📦 Data Structures (Composite Types)

| Type | Syntax | Mutable? | Ordered? | Unique Elements Only? |
| --- | --- | --- | --- | --- |
| **`list`** | `[1, 2, 3]` | ✅ Yes | ✅ Yes | ❌ No |
| **`tuple`** | `(1, 2, 3)` | ❌ No | ✅ Yes | ❌ No |
| **`set`** | `{1, 2, 3}` | ✅ Yes | ❌ No | ✅ Yes |
| **`dict`** | `{"key": "val"}` | ✅ Yes | ✅ Yes (3.7+) | Keys must be unique |

```python
# List (Ordered, mutable collection)
allowed_ips = ["192.168.1.1", "10.0.0.1"]

# Tuple (Immutable collection - useful for fixed configuration)
db_config = ("localhost", 5432, "admin_db")

# Set (Unordered, unique items only)
active_sessions = {"session_abc", "session_xyz"}

# Dictionary (Key-Value pairs)
user_profile = {
    "id": 101,
    "role": "analyst",
    "status": "active"
}

```

---

## 🔒 Security Perspective on Data Types

Handling types incorrectly can introduce subtle security flaws and application crashes:

### 1. Type Juggling & Injection Vulnerabilities

Python is strongly typed, so implicitly operating on mismatched types raises a `TypeError`. However, accepting raw user input without enforcing expected types can lead to logic bypasses.

```python
# SECURITY RISK: Assuming raw input is an integer
user_id_input = "105 OR 1=1"  # Malicious input string

# SAFE PRACTICE: Strict type conversion and validation
try:
    user_id = int(user_input)
except ValueError:
    # Handle malicious or invalid payload safely
    raise SecurityException("Invalid input format")

```

### 2. Mutability Risks

Mutable objects like `list` or `dict` passed into functions can be modified unexpectedly (side effects). Use immutable types like `tuple` for sensitive read-only values (e.g., system constants or user roles).

```python
# SECURE: Using tuples prevents accidental or unauthorized runtime modification
ALLOWED_ROLES = ("admin", "auditor", "analyst")

```

---

## ⚠️ Critical Warning: Floating Point Precision

> ⛔ **Never use standard `float` types for precise financial or cryptographic security calculations.**

Floats suffer from binary representation limitations:

```python
print(0.1 + 0.2)  # Output: 0.30000000000000004

```

For financial transactions or security tokens, always use the built-in **`decimal`** module or exact integers (e.g., storing values in cents).

```

```
