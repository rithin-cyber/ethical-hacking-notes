# Day 19 — Python Fundamentals Practice

**Date:** October 25, 2026  
**Topic:** Python fundamentals and core syntax

## 1. Variables and Data Types

Variables store data that a program can use.

```python
username = "labuser"
attempts = 3
is_admin = False

print(username)
print(attempts)
print(is_admin)
```

- `username` stores a string (`str`).
- `attempts` stores an integer (`int`).
- `is_admin` stores a Boolean (`bool`).
- Python uses `True` and `False`, with capital first letters.

## 2. Conditional Statements

`if` and `else` let a program make decisions.

```python
port = 443

if port == 443:
    print("HTTPS")
else:
    print("Unknown")
```

The program prints `HTTPS` because the port equals `443`.

## 3. Loops

A `for` loop repeats code for each item in a collection.

```python
ports = [22, 80, 443]

for port in ports:
    if port == 22:
        print("SSH")
    else:
        print(port)
```

Output:

```text
SSH
80
443
```

The loop processes each port in the list.

## 4. Functions and Return Values

Functions package reusable code. The `return` statement sends a value back to the code that called the function.

```python
def identify_service(port):
    if port == 22:
        return "SSH"
    return "Unknown"

service = identify_service(22)
print(service)
```

Output:

```text
SSH
```

The function checks the port and returns the corresponding service name.

## 5. Lists

Lists store multiple items in one variable. Python list indexing starts at `0`.

```python
ports = [22, 80, 443]

print(ports[1])
print(len(ports))
```

Output:

```text
80
3
```

- `ports[1]` accesses the second item.
- `len(ports)` returns the number of items.

## 6. Dictionaries

Dictionaries store information as key-value pairs.

```python
target = {"ip": "192.168.57.111", "port": 445}

print(target["ip"])
print(target["port"])
```

Output:

```text
192.168.57.111
445
```

- `target["ip"]` retrieves the IP address.
- `target["port"]` retrieves the port number.

Dictionaries are useful for organizing target details in security scripts.

## 7. Key Takeaways

- Variables store values of different data types.
- Conditions allow decisions based on comparisons.
- Loops process items repeatedly.
- Functions make code reusable and can return values.
- Lists store ordered collections of items.
- Dictionaries organize information using keys and values.
- Indentation is important in Python.

