```markdown
# Comments in Python & Security Best Practices

> **"Code tells you how; comments tell you why."**  
> Comments are essential for documenting code logic, clarifying complex algorithms, and maintaining security standards across software projects.

---

## 📌 Purpose of Comments

1. **Explain the "Why", Not the "How":** Python code is usually readable enough to explain *what* it does. Comments should clarify the intent, business logic, or reasoning behind specific design decisions.
2. **Code Maintainability:** Helps team members and future contributors quickly navigate, debug, and understand the codebase.
3. **Security Auditing & Annotations:** Highlight security checks, sanitization routines, potential risks, or areas requiring further review (e.g., `TODO` or `FIXME`).
4. **Debugging & Testing:** Temporarily disable specific lines or blocks of code during development without deleting them.

---

## 🛠️ Types of Comments & Code Examples

### 1. Single-Line Comments (`#`)
Used for concise explanations directly above or alongside the relevant line of code.

```python
# Check if the incoming request is coming from an authenticated administrator
if user.is_authenticated and user.is_admin:
    grant_access()

```

### 2. Multi-Line Comments & Docstrings (`"""`)

Used for detailed module overviews, function signatures, and technical documentation adhering to Python standards (PEP 257).

```python
def verify_hash(provided_password: str, stored_hash: str) -> bool:
    """
    Verifies a user's password against a stored bcrypt hash.

    Args:
        provided_password (str): The raw password string supplied by the user.
        stored_hash (str): The hashed baseline password from the database.

    Returns:
        bool: True if the password matches the hash, False otherwise.
    """
    return bcrypt.checkpw(provided_password.encode('utf-8'), stored_hash.encode('utf-8'))

```

---

## 🔒 Security Perspective on Comments

When developing security utilities, penetration testing scripts, or core application logic, comments play a pivotal role:

### 🟢 Good Security Commenting Practices:

* **Highlight Sanitization Routines:** Document where and why user inputs are validated or encoded to prevent injection vulnerabilities.
* **Track Security Backlog Items:** Tag areas that need cryptographic updates or security compliance checks.

```python
# SECURITY: Sanitizing input parameters to mitigate SQL Injection risks
clean_input = re.sub(r'[^a-zA-Z0-9]', '', user_input)

# TODO: Replace hardcoded API timeout with an environment variable prior to deployment
TIMEOUT_SECONDS = 30

```

---

## ⚠️ Critical Warning: What NOT to Include in Comments

> ⛔ **Never leave confidential data or sensitive internal logic inside public or private comments.**

* **No Credentials:** Never leak hardcoded passwords, API tokens, JWT secrets, or connection strings.
* **No Unmasked Flaws:** Avoid detailing active bypasses or security flaws (e.g., `# FIXME: Bypassing authentication for testing`). Attackers scanning repositories actively target these markers.

```

```
