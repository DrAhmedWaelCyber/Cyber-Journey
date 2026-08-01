```markdown
# Handling User Input (`input()`) in Python & Security Considerations

> **"Never trust user input. All input is untrusted and malicious until strictly validated and sanitized."**  
> The `input()` function pauses program execution and reads a line of input from standard input (`stdin`) as a string. In security engineering and tool development, raw input handling is the frontline of defense against injection attacks, buffer overruns, and application crashes.

---

## 📌 Fundamentals of the `input()` Function

In Python 3, `input()` **always returns a string (`str`)**, regardless of what the user types.

```python
# Reading standard input
target_ip = input("Enter target IP address: ").strip()

print(f"[*] Target locked: {target_ip}")

```

### Type Conversion

Because `input()` returns a string, numerical inputs must be explicitly cast using `int()`, `float()`, etc.

```python
# Explicit type casting
raw_port = input("Enter target port [1-65535]: ")
port = int(raw_port)  # Raises ValueError if the user enters non-numeric text!

```

---

## 🛠️ Safe Input Validation & Error Handling

To prevent application crashes when users (or automated tools) provide invalid input formats, wrap conversions inside a `try-except` block.

```python
def get_valid_port() -> int:
    while True:
        user_input = input("Enter port number: ").strip()
        
        try:
            port = int(user_input)
            # Enforce range boundaries
            if 1 <= port <= 65535:
                return port
            print("[-] Port must be between 1 and 65535.")
        except ValueError:
            print("[-] Invalid input. Please enter a valid integer.")

# Usage
target_port = get_valid_port()

```

---

## 🔒 Security Perspective on User Input

Improperly handled inputs are the primary source of vulnerabilities in software systems (OWASP Top 10).

### 1. Python 2 `input()` vs. Python 3 `input()` Vulnerability

In deprecated **Python 2**, `input()` evaluated the input string as Python code (`eval(raw_input())`), allowing arbitrary code execution. Python 3 fixed this by treating `input()` strictly as `raw_input()`.

> ⛔ **Never pass user input directly into `eval()` or `exec()` in Python 3!**

```python
# ⛔ CRITICAL VULNERABILITY: Arbitrary Code Execution
user_expr = input("Enter formula: ")
# result = eval(user_expr)  # Attacker inputs __import__('os').system('sh') to gain shell access!

```

### 2. Masking Sensitive Input (Passwords / API Keys)

Using standard `input()` to read secrets leaves passwords visible on the screen in plain text, making them vulnerable to shoulder surfing and terminal logging. Use the built-in **`getpass`** module instead.

```python
import getpass

# ⛔ INSECURE: Echoes raw password characters to terminal output
# password = input("Enter password: ")

# 🟢 SECURE: Suppresses console output while typing
password = getpass.getpass("Enter secret API token: ")

```

### 3. Non-Interactive Scripts & `stdin` Pipelines

In security automation, inputs are often piped directly from other command-line utilities (e.g., `cat subdomains.txt | python3 scanner.py`). Use `sys.stdin` to handle piped data safely.

```python
import sys

# Check if input is being piped via stdin
if not sys.stdin.isatty():
    print("[*] Processing piped input stream...")
    for line in sys.stdin:
        target = line.strip()
        if target:
            print(f"[+] Scanning piped target: {target}")
else:
    target = input("Enter target manually: ").strip()

```

---

## ⚠️ Critical Warning: Input Sanitization before Execution

> ⛔ **Passing raw strings from `input()` directly into system calls (`os.system`), SQL queries, or file paths creates high-severity vulnerabilities.**

```python
import subprocess
import shlex

raw_host = input("Enter hostname to ping: ")

# ⛔ VULNERABLE to Command Injection:
# os.system("ping -c 1 " + raw_host)

# 🟢 SECURE: Sanitize input parameters or split using shlex
safe_args = shlex.split(raw_host)
subprocess.run(["ping", "-c", "1"] + safe_args, shell=False)

```

```

```
