# Python Scripting Foundations

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)
![Category](https://img.shields.io/badge/Focus-Automation%20%7C%20SOC%20Scripting-green)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

This repository exists as a fast, practical reference for writing clean, modern Python, especially for automation, SOC tooling, and daily scripting tasks. If you're like me and would rather focuses on **shortcuts, cleaner patterns, and “better ways”** to write scripts right from the beginning, this is for you.

Think of this as a cheat sheet for writing smarter, more expressive Python.

---

## Table of Contents
- [Introduction](#python-scripting-foundations)
- [Variables](#variables)
- [Lists](#lists)
- [Dictionaries](#dictionaries)
- [If / Else](#if--else)
- [Loops](#loops)
- [Functions](#functions)
- [Try / Except](#try--except)
- [File I/O](#file-io)
- [Virtual Environments](#virtual-environments)
- [Imports](#imports)
- [Sample Script (Challenge)](#sample-script-challenge)
- [SOC Automation Example](#soc-automation-example)
- [Final Thoughts](#final-thoughts)
- [Next Steps](#next-steps)

---

### “We start with humble beginnings…”

`````````py
print("Hello, world.")
# Basic print — works, but doesn't scale when you need variables or formatting.
`````````

Now, the improved versions:

`````````py
name = "Michael"

print(f"Hello, {name}.")           
# f-strings: fastest, cleanest, and recommended.

print("Hello, {}".format(name))    
# Older .format() — works, more typing.

print("Hello,", name)              
# Python auto-inserts spaces.
`````````

---

## Variables

```py
x = 5
y = 10
z = 15
# Standard variable assignments. Python creates variables as soon as you assign them.
```

**Multiple assignment (tuple unpacking)**  
Assign several variables at once in a single line.

```py
a, b, c = 5, 10, 15
```

**Swapping values**  
A Python trick to swap variables without using a temporary placeholder.

```py
left, right = right, left
```

**Unpacking with a “catch-all”**  
The starred variable grabs any number of values.

```py
first, *middle, last = [1, 2, 3, 4, 5]
# middle becomes [2, 3, 4]
```

**Unpacking ranges**  
A quick way to generate sequential numbers and assign them immediately.

```py
a, b, c = range(3)  # 0, 1, 2
```


---

## Lists

```py
servers = ["splunk", "pfsense", "core-switch"]
# A list is an ordered, changeable collection.
```

**List expansion**  
Two clean ways to add multiple items.

```py
servers += ["kali"]
servers.extend(["ids", "honeypot"])
```

**List comprehension**  
A compact way to build or transform lists.

```py
pings = [f"Pinging {srv}..." for srv in servers]
# Reads like: “for each server, build this string.”
```

**Slicing**  
Extracts a portion of a list using index ranges.

```py
subset = servers[1:3]
# Grabs indexes 1 and 2.
```

**Reverse iteration**

```py
for srv in reversed(servers):
    print(srv)
# reversed(...) does not copy the list; it just reverses the view.
```


---

## Dictionaries

```py
user = {"username": "michael", "role": "analyst"}
# Dictionaries store key-value pairs, like mini JSON objects.
```

**Merging dictionaries**  
A clean way to combine or override keys.

```py
full_user = {**user, "privileges": ["read", "write"]}
```

**Inline dictionary creation**

```py
config = dict(port=8080, retries=5, secure=True)
# A stylistic alternative to {...}.
```

**Looping through key-value pairs**

```py
for key, value in user.items():
    print(key, value)
```

**Safe key access**  
Avoids errors if the key doesn't exist.

```py
role = user.get("role", "unknown")
```


---

## If / Else

```py
if x > 5:
    print("Big number")
else:
    print("Small number")
# Standard conditional.
```

**One-line conditional**  
Useful for compact expressions.

```py
print("Big") if x > 5 else print("Small")
```

**Truthiness**  
Many Python objects evaluate to True or False by default.

```py
if servers:  # A non-empty list = True
    print("Servers exist")
```


---

## Loops

```py
for srv in servers:
    print(srv)
# Basic iteration through a list.
```

**Enumerate**  
Adds an automatic counter.

```py
for i, srv in enumerate(servers, start=1):
    print(i, srv)
```

**Zip iteration**  
Pairs elements from multiple lists.

```py
for name, port in zip(servers, [514, 443, 8000]):
    print(name, port)
```

**Filtered list comprehension**

```py
critical = [srv for srv in servers if "splunk" in srv]
# Creates a new list only containing items that match the condition.
```


---

## Functions

```py
def greet(name):
    return f"Hello, {name}"
# A basic function that returns a value.
```

**Default parameters**

```py
def greet(name="stranger"):
    return f"Hello, {name}"
# If no name is provided, "stranger" is used.
```

**Accept any number of positional arguments**

```py
def audit(*hosts):
    for h in hosts:
        print("Auditing", h)
# *hosts collects all positional arguments into a tuple.
```

**Accept any number of keyword arguments**

```py
def configure(**settings):
    return settings
# **settings collects named arguments into a dictionary.
```

**Returning multiple values**

```py
def bounds():
    return 10, 20

low, high = bounds()
# Python automatically unpacks the returned tuple.
```


---

## Try / Except

```py
try:
    risky = 1 / 0
except ZeroDivisionError:
    print("Math said no.")
# try blocks watch for errors; except handles specific ones.
```

**Catching multiple exceptions**

```py
try:
    do_something()
except (ValueError, TypeError):
    print("Input problem")
```

**Reraising with context**

```py
try:
    risky()
except Exception as e:
    raise RuntimeError("Something exploded") from e
# 'from e' preserves the original traceback.
```


---

## File I/O

```py
with open("notes.txt", "w") as f:
    f.write("Document everything.")
# 'with' ensures the file closes automatically.
```

**Using pathlib (cleaner, modern approach)**

```py
from pathlib import Path
Path("logs.txt").write_text("Log1\nLog2")
```

**Reading using pathlib**

```py
lines = Path("logs.txt").read_text().splitlines()
# splitlines() removes newline characters.
```


---

## Virtual Environments

```py
python -m venv venv
# Creates an isolated Python environment.
```

Activate on Windows:

```py
venv\Scripts\activate
```

Activate on Linux/macOS:

```py
source venv/bin/activate
```


---

## Imports

```py
import os, sys
# Standard library imports.
```

```py
from datetime import datetime, timedelta
# Import specific objects from a module.
```

```py
from pathlib import Path
from collections import defaultdict
# Helpful modern utilities for file paths and counting/grouping.
```

---

## Sample Script (Challenge)

Now that you've seen each concept individually, here they are combined into a mini automation script.

**Challenge:**  
Before running it, see if you can figure out:
1. What it prints  
2. What file it creates  
3. Where the error occurs  
4. What ends up in the log  

`````````py
from pathlib import Path
from datetime import datetime

server1, server2, server3 = "splunk", "pfsense", "kali"
servers = [server1, server2, server3]

base_config = {"mode": "secure"}
extra_config = {"retries": 3, "timeout": 10}
config = {**base_config, **extra_config}

def audit(*hosts):
    for index, host in enumerate(hosts, start=1):
        print(f"{index}. Auditing {host}...")
    return f"Finished auditing {len(hosts)} host(s)."

def generate_report(**details):
    timestamp = datetime.now().isoformat()
    return f"{timestamp} REPORT {details}"

def risky_operation():
    try:
        return 10 / 0
    except Exception as e:
        raise RuntimeError("risky_operation failed") from e

def main():
    print("=== Demo Script Starting ===")

    statuses = [f"Pinging {srv}..." for srv in servers]
    print("Statuses generated:", statuses)

    audit_result = audit(*servers)
    print(audit_result)

    if servers:
        print("Server list is not empty.")

    log_path = Path("automation_log.txt")
    data = generate_report(action="audit", targets=servers, config=config)
    log_path.write_text(data)
    print("Report written to:", log_path.resolve())

    try:
        risky_operation()
    except RuntimeError as err:
        print("Error caught:", err)

    print("=== Demo Script Complete ===")

if __name__ == "__main__":
    main()
`````````

---

## SOC Automation Example

Now that you've seen a general-purpose automation script, here’s how these exact Python techniques translate into **real cybersecurity work**.

This example simulates a lightweight SOC automation tool that:

- Reads a log file  
- Detects multiple failed logins from the same IP  
- Flags potential brute force attacks  
- Writes findings to a report  

`````````py
from pathlib import Path
from collections import defaultdict
from datetime import datetime

log_file = Path("auth.log")

# Sample log contents (the script creates the file itself)
sample_logs = """\
2025-01-01 10:10:01 Failed login from 192.168.1.10
2025-01-01 10:10:02 Failed login from 192.168.1.10
2025-01-01 10:10:05 Failed login from 10.0.0.5
2025-01-01 10:10:07 Failed login from 192.168.1.10
"""

log_file.write_text(sample_logs)

# Count failed attempts per IP
failures = defaultdict(int)

for line in log_file.read_text().splitlines():
    if "Failed login" in line:
        ip = line.split()[-1]
        failures[ip] += 1

# Build report using earlier techniques
alerts = []
for ip, count in failures.items():
    if count >= 3:
        alerts.append(f"Brute force suspected from {ip} ({count} failures)")

report = Path("soc_report.txt")
report.write_text("\n".join(alerts) or "No threats detected.")

print("SOC Report Generated:", report.resolve())
`````````

### Why this matters  
This tiny example demonstrates concepts used constantly in SOC environments:

- Log parsing  
- Pattern detection  
- Aggregation  
- Threshold-based alerting  
- Automation to reduce analyst workload  

It also uses every Python technique you learned earlier, showing how foundational scripting directly powers practical cybersecurity automation.

---

## Final Thoughts

This guide isn’t meant to teach Python from the ground up.  
It’s meant to make you **dangerous quickly** — to give you the expressive tools that experienced engineers use by default.

Use these patterns as your baseline.  
Everything you build from here becomes clearer, cleaner, and more maintainable.

---

## Next Steps

This repo is part of a broader scripting foundation series.  
Check out the upcoming **Bash** and **PowerShell** versions for matching automation patterns across languages.
