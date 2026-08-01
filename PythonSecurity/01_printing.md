```markdown
# Mastering Printing and Output Formatting in Python

When I started diving deeper into Python for cybersecurity and tool development, I quickly realized that printing isn't just about displaying text on a terminal screen. Output formatting is a core skill—whether you are building interactive CLI tools, formatting report outputs, debugging network scripts, or aligning log data neatly.

In this article, I will walk through everything you need to know about console output in Python, covering standard printing techniques, escape characters, advanced string formatting, and redirecting outputs.

---

## 1. The Core `print()` Function

At its simplest level, Python’s `print()` function takes an object, converts it into a string, and outputs it to the console.

```python
print("Starting the system diagnostic...")

```

By default, every time you call `print()`, Python appends a newline character at the end and separates multiple arguments with a space.

---

## 2. Taking Control: Custom Separators and Endings

Python allows us to modify how arguments are joined and how the line terminates using the `sep` and `end` keyword parameters.

### Customizing the Separator (`sep`)

If you pass multiple items to `print()`, you can choose what goes between them instead of the default space. This is especially useful for formatting IP addresses or paths:

```python
# Constructing an IP address
print("192", "168", "1", "100", sep=".")
# Output: 192.168.1.100

```

### Controlling Line Endings (`end`)

By default, `end="\n"`. If you want to keep the terminal cursor on the same line—for instance, when showing progress—you can override it:

```python
print("Checking database connection...", end=" ")
print("[OK]")
# Output: Checking database connection... [OK]

```

---

## 3. Formatting with Escape Characters

Escape characters let you insert special formatting within strings using a backslash (`\`). Here are the most essential ones I use regularly:

* `\n`: Inserts a new line.
* `\t`: Inserts a tab stop for quick alignment.
* `\\`: Prints a literal backslash.
* `\"` or `\'`: Prints quotation marks inside a string without breaking it.

```python
# Combining escape characters for structured terminal output
print("Scan Summary:\n\t- Target: 10.0.0.1\n\t- Open Ports: 80, 443")

```

---

## 4. Modern String Formatting with F-Strings

While Python has older formatting methods (like using `%` or `.format()`), **f-strings (Formatted String Literals)** introduced in Python 3.6 are the gold standard. They are faster, cleaner, and much easier to write.

To use an f-string, simply prefix the string with `f` or `F` and place variables or expressions inside curly braces `{}`.

```python
target_host = "10.0.0.15"
port = 443
status = "Open"

# Basic Variable Interpolation
print(f"[*] Port {port} on {target_host} is currently {status}.")

# Evaluating expressions inside f-strings
print(f"[*] Next sequential port to test: {port + 1}")
print(f"[*] Upper case status: {status.upper()}")

```

---

## 5. Advanced F-String Precision and Alignment Rules

F-strings really shine when you need fine control over numerical output, floating-point precision, or text alignment in terminal tables.

### Floating-Point Precision

You can control the number of decimal places using the `:.Nf` syntax:

```python
execution_time = 1.489234
print(f"Scan completed in {execution_time:.2f} seconds.")
# Output: Scan completed in 1.49 seconds.

```

### Text Alignment and Padding

You can align text left (`<`), right (`>`), or center (`^`) while specifying a total width. This is key for creating clean terminal reports:

```python
# Structuring a quick header
print(f"{'Service':<15} | {'Port':^10} | {'Status':>10}")
print("-" * 41)
print(f"{'HTTP':<15} | {'80':^10} | {'Active':>10}")
print(f"{'SSH':<15} | {'22':^10} | {'Closed':>10}")

```

### Binary and Hexadecimal Conversions

In low-level scripting or security work, converting numbers to binary or hex formats directly inside f-strings saves a lot of time:

```python
val = 255
print(f"Decimal: {val} | Hex: {val:x} | Binary: {val:b}")
# Output: Decimal: 255 | Hex: ff | Binary: 11111111

```

---

## 6. Output Redirection: Writing to Files

`print()` isn't limited to the terminal screen. You can pass a file object to the `file` parameter to direct logs straight to a file on disk:

```python
with open("scan_results.txt", "w") as log_file:
    print("[+] Scan started successfully.", file=log_file)
    print("[+] Target host reached.", file=log_file)

```

---

## Summary Best Practices

1. **Prefer F-Strings:** Use f-strings for all dynamic string operations—they are readable, maintainable, and highly efficient.
2. **Use `sep` & `end` for CLI UX:** Clean up terminal outputs by eliminating unnecessary line breaks or extra spaces.
3. **Keep Code Readable:** For complex formatted tables, break down long f-string lines to keep your scripts clean and readable.

```

```
