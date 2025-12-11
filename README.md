# Python Scripting Foundations

### *“We start with humble beginnings…”*

```py
print("Hello, world.")
# Basic print — works, but doesn't scale when you need variables or formatting.
```

Now, the improved versions:

```py
name = "Michael"

print(f"Hello, {name}.")           
# f-strings: fastest, cleanest, and readable. Recommended for almost everything.

print("Hello, {}".format(name))    
# Older `.format()` style — still valid, just more typing.

print("Hello,", name)              
# Python auto-inserts spaces. Works, but formatting gets messy quickly.
```

---

## Variables
Basic assignments:

```py
x = 5
y = 10
z = 15
# Standard assignments — clear but repetitive.
```

Better: **Multiple assignment (tuple unpacking)**

```py
a, b, c = 5, 10, 15
# Assign multiple variables at once — Python unpacks the values automatically.
```

Swapping without a temporary variable:

```py
left, right = right, left
# Python swaps both values simultaneously — elegant and avoids temp vars.
```

Capturing the "middle" section of a list:

```py
first, *middle, last = [1, 2, 3, 4, 5]
# *middle captures any number of items. Powerful pattern for parsing lists.
```

---

## Lists
Basic:

```py
servers = ["splunk", "pfsense", "core-switch"]
```

Better list operations:

```py
servers += ["kali"]                    
# Adds items without calling append() repeatedly — cleaner for multiple values.

servers.extend(["ids", "honeypot"])    
# Efficient for bulk additions — faster and more explicit than append in loops.
```

List comprehension (the Python flex):

```py
pings = [f"Pinging {srv}..." for srv in servers]
# Builds a new list in one readable expression — efficient + concise.
```

---

## Dictionaries
Basic:

```py
user = {
    "username": "michael",
    "role": "analyst"
}
```

Merge dictionaries (clean and powerful):

```py
full_user = {**user, "privileges": ["read", "write"]}
# ** expands dictionary pairs — lets you merge or override fields easily.
```

Inline dictionary creation:

```py
config = dict(port=8080, retries=5, secure=True)
# Same as {"port": 8080, ...} but cleaner when building configs dynamically.
```

Looping properly:

```py
for key, value in user.items():
    print(f"{key}: {value}")
# .items() gives pairs — keeps code clean and readable.
```

---

## If / Else
Basic:

```py
if x > 5:
    print("Big number")
else:
    print("Small number")
```

One-liner:

```py
print("Big number") if x > 5 else print("Small number")
# Good for compact logic — but don’t overuse it.
```

Using Python “truthiness”:

```py
if servers:     
    print("We have servers.")
# Non-empty lists evaluate to True. Empty lists = False.
```

---

## Loops
Basic:

```py
for srv in servers:
    print(srv)
```

Better: numbered iteration:

```py
for i, srv in enumerate(servers, start=1):
    print(f"{i}. {srv}")
# enumerate() gives you both index and value — more control, cleaner code.
```

Looping through two lists at once:

```py
for name, port in zip(servers, [514, 443, 8000]):
    print(name, port)
# zip() pairs items — avoids using range() or indexing into lists manually.
```

List comprehension filtering:

```py
critical = [srv for srv in servers if "splunk" in srv]
# Builds a filtered list inline — very Pythonic.
```

---

## Functions
Basic:

```py
def greet(name):
    return f"Hello, {name}"
```

Better: default parameter:

```py
def greet(name="stranger"):
    return f"Hello, {name}"
# Default values prevent errors when arguments are missing.
```

Flexible argument handling:

```py
def audit(*hosts):
    # *hosts packs all positional arguments into a tuple.
    for h in hosts:
        print(f"Auditing {h}...")

audit("fw01", "fw02", "dmz-switch")
```

Keyword argument flexibility:

```py
def configure(**settings):
    # **settings packs keyword arguments into a dictionary.
    return settings

cfg = configure(mode="secure", retries=5, encryption="AES")
```

---

## Try / Except
Basic:

```py
try:
    risky = 1 / 0
except ZeroDivisionError:
    print("Math said no.")
```

Catching multiple errors:

```py
try:
    do_something()
except (ValueError, TypeError) as e:
    print(f"Bad input: {e}")
# Keeps exception handling tidy when multiple issues share the same response.
```

Chaining exceptions:

```py
try:
    risky()
except Exception as e:
    raise RuntimeError("Something exploded.") from e
# `from e` keeps the original traceback — this is what senior engineers do.
```

---

## File I/O
Basic:

```py
with open("notes.txt", "w") as f:
    f.write("Document everything.")
# `with` auto-closes the file even if errors occur — always use it.
```

Better: using pathlib (cleanest modern approach):

```py
from pathlib import Path

lines = ["Log1", "Log2", "Log3"]
Path("logs.txt").write_text("\n".join(lines))
# Path.write_text handles opening, writing, and closing under the hood.
```

Reading cleanly:

```py
data = Path("logs.txt").read_text().splitlines()
# splitlines() removes newlines and returns a clean list of strings.
```

---

## Virtual Environments
Basic:

```py
python -m venv venv
# Creates a self-contained Python environment.
```

Activating:

```py
venv\Scripts\activate     
# Windows

source venv/bin/activate  
# Linux/macOS
```

Upgrade pip & check packages:

```py
pip install --upgrade pip
pip list
# Good hygiene for fresh environments.
```

---

## Imports
Basic imports:

```py
import os
import sys
```

Grouped imports:

```py
from datetime import datetime, timedelta
# Import multiple symbols from the same module cleanly.
```

Useful modern modules:

```py
from pathlib import Path           
# Modern filesystem handling — preferred over os.path

from collections import defaultdict  
# Auto-creates empty values — amazing for building counters or grouped data.
```

---

## Sample Script

```py
from pathlib import Path
from datetime import datetime

# Multiple assignment (clean way to assign many variables at once)
server1, server2, server3 = "splunk", "pfsense", "kali"

# Store servers in a list for loops and comprehension
servers = [server1, server2, server3]

# Dictionary merging using ** unpacking
base_config = {"mode": "secure"}
extra_config = {"retries": 3, "timeout": 10}
config = {**base_config, **extra_config}

# Function that accepts any number of positional arguments using *args
def audit(*hosts):
    for index, host in enumerate(hosts, start=1):
        print(f"{index}. Auditing {host}...")
    return f"Finished auditing {len(hosts)} host(s)."

# Function that accepts keyword arguments using **kwargs
def generate_report(**details):
    timestamp = datetime.now().isoformat()
    return f"{timestamp} REPORT {details}"

# Demonstrates exception handling and chained errors
def risky_operation():
    # This always fails to show how try/except works
    try:
        return 10 / 0
    except Exception as e:
        # Re-raise with context
        raise RuntimeError("risky_operation failed") from e

def main():

    print("=== Demo Script Starting ===")

    # List comprehension that builds a status list
    statuses = [f"Pinging {srv}..." for srv in servers]
    print("Statuses generated:", statuses)

    # Run audit using *servers to unpack the list
    audit_result = audit(*servers)
    print(audit_result)

    # Using truthiness of lists (non-empty means True)
    if servers:
        print("Server list is not empty.")

    # Create a log file using pathlib (file is auto-created)
    log_path = Path("automation_log.txt")
    report_data = generate_report(action="audit", targets=servers, config=config)
    log_path.write_text(report_data)
    print(f"Report written to {log_path.resolve()}")

    # Demonstrate error handling
    try:
        risky_operation()
    except RuntimeError as err:
        print("Error caught:", err)

    print("=== Demo Script Complete ===")

# Only execute if run directly
if __name__ == "__main__":
    main()
```

---

## Final Thoughts
This repo skips the slow on-ramp and jumps straight into:
• “Here’s the basic way.”  
• “Here’s the better way.”  
• “Here’s the Pythonic way.”  

Short, clear, and full of power-user tricks you’ll actually use when scripting for automation, SOC tooling, detection engineering, or just making your life easier.

If you want, I can now generate:
• A repo structure  
• Example automation scripts  
• Recommended learning path  
• Bash + PowerShell versions in the same style  
