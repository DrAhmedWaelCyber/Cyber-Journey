```markdown
# Functions in Python & Security Considerations

> **"Functions enforce modularity, lessen code duplication, and establish clear execution boundaries. Poorly validated function arguments and weak return handling are major sources of logic flaws."**  
> Functions are reusable blocks of code that execute when called. In cybersecurity tooling, functions encapsulate discrete operational tasks—such as parsing network headers, authenticating tokens, or calculating cryptographic hashes.

---

## 📌 Fundamentals of Python Functions

Functions are defined using the `def` keyword, followed by the function name, parameters in parentheses, and an indented block.

```python
# Defining a basic function with parameters and return values
def calculate_hash_length(algorithm: str) -> int:
    """Returns the expected output length in hex characters for a hash algorithm."""
    algorithm_map = {
        "md5": 32,
        "sha1": 40,
        "sha256": 64,
        "sha512": 128
    }
    return algorithm_map.get(algorithm.lower(), 0)

# Calling the function
length = calculate_hash_length("sha256")  # Returns 64

```

---

## 🛠️ Positional, Keyword, and Default Arguments

### 1. Default Argument Values

Provide fallback values for optional parameters.

```python
def scan_target(host: str, port: int = 80, timeout: float = 2.0) -> bool:
    print(f"[*] Scanning {host}:{port} with a {timeout}s timeout...")
    # Network scan logic here
    return True

# Call with default port and timeout
scan_target("192.168.1.1")

# Call with explicit keyword arguments
scan_target(host="10.0.0.1", port=443, timeout=5.0)

```

### 2. Variable-Length Arguments (`*args` and `**kwargs`)

* **`*args`**: Captures extra positional arguments as a tuple.
* **`**kwargs`**: Captures extra keyword arguments as a dictionary.

```python
def log_security_event(event_type: str, *tags, **metadata):
    print(f"[!] Event: {event_type}")
    print(f"    Tags: {tags}")
    print(f"    Metadata: {metadata}")

log_security_event("FAILED_LOGIN", "auth", "brute_force", user="admin", ip="192.168.1.50")

```

---

## 🔒 Security Perspective on Functions

Structuring functions securely protects applications against side-effects, mutable parameter corruption, and scope leaks.

### 1. The Mutable Default Argument Trap

Setting mutable objects (like lists or dictionaries) as default parameter values is a well-known Python vulnerability. Default parameter values are evaluated **only once when the function is defined**, not every time it is called. This shares state across calls!

```python
# ⛔ VULNERABLE: Shared mutable state across calls
def add_target_bad(ip: str, target_list=[]):
    target_list.append(ip)
    return target_list

# Calling add_target_bad("10.0.0.1") modifies the shared list for all future callers!

# 🟢 SECURE: Use None as default and initialize inside the function
def add_target_secure(ip: str, target_list: list = None) -> list:
    if target_list is None:
        target_list = []
    target_list.append(ip)
    return target_list

```

### 2. Enforcing Keyword-Only Arguments for Critical Security Flags

When functions accept sensitive flags (e.g., `verify_ssl` or `dry_run`), enforce **keyword-only arguments** using `*` in the parameter list to prevent caller confusion or ordering mistakes.

```python
# 🟢 SECURE: Parameters after '*' MUST be passed by name
def execute_request(url: str, *, verify_ssl: bool = True, timeout: int = 10):
    if not verify_ssl:
        print("[!] WARNING: SSL verification disabled!")
    # HTTP request logic

# execute_request("[https://target.com](https://target.com)", False)  # Raises TypeError!
execute_request("[https://target.com](https://target.com)", verify_ssl=False)  # Allowed

```

### 3. Type Hints and Input Guarding

Python runtime does not strictly enforce type hints, so functions processing untrusted input must still perform runtime validation.

```python
def set_user_role(user_id: int, role: str) -> None:
    # Guard clause against malformed runtime input
    if not isinstance(user_id, int) or user_id <= 0:
        raise ValueError("Invalid user ID")
    
    allowed_roles = {"guest", "analyst", "admin"}
    if role not in allowed_roles:
        raise PermissionError(f"Unauthorized role assignment attempt: {role}")
    
    # Process role assignment

```

---

## ⚠️ Critical Warning: Lambda Functions in Security Logic

> ⛔ **Avoid using `lambda` (anonymous inline functions) for complex security routines, authentication checks, or input sanitization. Lambdas lack docstrings, type annotations, and explicit stack traces, making them hard to audit.**

```python
# ⛔ POOR PRACTICE: Obscured security logic inside a lambda
check_admin = lambda user: True if user.get("role") == "admin" and user.get("mfa") else False

# 🟢 SECURE & AUDITABLE: Explicit named function
def is_admin_authenticated(user: dict) -> bool:
    if not isinstance(user, dict):
        return False
    return user.get("role") == "admin" and bool(user.get("mfa"))

```

```

<FollowUp label="Want to create the next file on Scope & Namespaces?" query="Write the content for the next file in the series: Scope and Namespaces in Python."/>

```
