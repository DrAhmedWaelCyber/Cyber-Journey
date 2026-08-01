```markdown
# Variables & Constants in Python (Security Perspective)

> **"Proper variable management prevents memory leaks, scope pollution, and state tampering."**  
> Understanding how variables, constants, and scope work in Python is essential for writing clean code and secure security automation tools.

---

## 📌 Variables in Python

A variable is a named container that holds a reference to an object in memory.

### Key Characteristics:
* **Dynamic Binding:** Variables don't require static type declarations; they point to whichever object is assigned to them.
* **Reassignment:** A variable name can be reassigned to a completely different type at runtime.

```python
target_host = "192.168.1.100"  # Points to a string
target_host = 8080             # Reassigned to an integer

```

---

## 📌 Constants in Python

Unlike languages such as C++ or JavaScript (`const`), Python **does not have built-in language enforcement for constants**.

### Naming Convention (PEP 8):

By convention, developers use **ALL_CAPS** to signal that a variable should be treated as read-only.

```python
# Intended as constants — should NOT be modified at runtime
MAX_FAILED_ATTEMPTS = 5
DEFAULT_PORT = 443
API_BASE_URL = "[https://api.security-scanner.internal/v1](https://api.security-scanner.internal/v1)"

```

---

## 🛠️ Variable Scope & Lifecycle

Understanding scope prevents unintended state overrides and security bugs.

| Scope | Description | Best Practice |
| --- | --- | --- |
| **Local** | Declared inside a function; exists only during function execution. | Prefer local scope to keep memory clean and prevent leakage. |
| **Global** | Declared at the top level of a module; accessible across the file. | Minimize global state to avoid race conditions and parameter tampering. |

```python
global_log_file = "/var/log/scanner.log"  # Global scope

def run_scan(target: str):
    scan_status = "RUNNING"  # Local scope to run_scan()
    print(f"[{scan_status}] Scanning {target} -> Logging to {global_log_file}")

```

---

## 🔒 Security Perspective on Variables & Constants

Variables directly interact with application memory and application state. Improper handling introduces key security risks:

### 1. Insecure Constant Handling (Environment Variables)

Hardcoding sensitive configuration constants (like API tokens or secret keys) directly into your script exposes them in version control (e.g., GitHub).

```python
# ⛔ INSECURE: Hardcoded secret constant
JWT_SECRET = "super_secret_key_12345"

# 🟢 SECURE: Load sensitive constants dynamically from environment variables
import os

JWT_SECRET = os.getenv("JWT_SECRET_KEY")
if not JWT_SECRET:
    raise ValueError("CRITICAL: JWT_SECRET_KEY environment variable is missing!")

```

### 2. Variable State Tampering & Race Conditions

If global variables track security states (such as `is_admin` or `session_active`), concurrent requests or improper function calls might alter them globally, granting unauthorized access to other sessions.

```python
# ⛔ INSECURE: Using a shared global variable for request-specific state
current_user_authenticated = False

def authenticate_user(user_token):
    global current_user_authenticated
    if validate_token(user_token):
        current_user_authenticated = True  # Risk: Pollutes global state for subsequent runs

```

### 3. Sensitive Data Persistence in Memory

Python's garbage collector frees unused memory periodically, but sensitive strings (like raw passwords or decrypted tokens) remain in memory as long as a variable reference exists.

```python
# 🟢 SECURE: Clear sensitive variable references immediately after use
raw_password = get_user_input()
hashed_password = hash_function(raw_password)

# Explicitly delete the reference to reduce the in-memory exposure window
del raw_password

```

---

## ⚠️ Critical Warning: Mutability of Constant Collections

> ⛔ **Declaring a list or dictionary in ALL_CAPS does NOT make its internal items immutable.**

```python
# ALL_CAPS signals intent, but Python still permits mutation:
ALLOWED_ADMINS = ["alice", "bob"]
ALLOWED_ADMINS.append("malicious_user")  # Python permits this!

# 🟢 SECURE: Use an immutable tuple for read-only constant collections
ALLOWED_ADMINS = ("alice", "bob")
# ALLOWED_ADMINS.append("malicious_user")  # Raises AttributeError

```

```

```
