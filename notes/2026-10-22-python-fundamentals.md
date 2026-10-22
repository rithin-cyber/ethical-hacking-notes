# Day 16 — Python Fundamentals

**Date:** October 22, 2026  
**Topic:** Python Fundamentals  
**Project:** Port Classification Script  
**Language:** Python 3

## 1. Objective

Learn Python fundamentals and use them to build a simple script that classifies common network port numbers by their usual service associations.

## 2. Python Concepts Practiced

- **Variables:** Store values for later use.
- **Lists:** Store multiple port numbers in one variable.
- **Functions:** Organize reusable code into a named block.
- **Conditional statements:** Use `if`, `elif`, and `else` to make decisions.
- **Loops:** Process each port in a list.
- **Return values:** Send a result from a function back to the code that called it.
- **Printing:** Display results using `print()`.

## 3. Script

**Filename:** `port_info.py`

```python
def identify_service(port):
    if port == 21:
        return "FTP"
    elif port == 22:
        return "SSH"
    elif port == 80:
        return "HTTP"
    elif port == 443:
        return "HTTPS"
    elif port == 8080:
        return "HTTP-Alt"
    elif port == 3306:
        return "MySQL"
    else:
        return "Unknown service"


ports = [21, 22, 80, 443, 8080, 3306]

for port in ports:
    service = identify_service(port)
    print("Port", port, ":", service)
```

## 4. How the Script Works

### Step 1: Define the function

The function `identify_service(port)` receives a port number and checks which conditions match.

### Step 2: Check conditions

The `if`, `elif`, and `else` statements determine which service name to return.

For example, when the port is `22`, the function returns `"SSH"`.

### Step 3: Store port numbers

The list `ports` contains six port numbers that the script will process.

### Step 4: Process each port

The `for` loop takes one port from the list at a time and passes it to `identify_service()`.

### Step 5: Store the returned value

The line `service = identify_service(port)` stores the value returned by the function in the `service` variable.

### Step 6: Display the result

The `print()` statement displays the current port number and its associated service name.

## 5. Example Output

```text
Port 21 : FTP
Port 22 : SSH
Port 80 : HTTP
Port 443 : HTTPS
Port 8080 : HTTP-Alt
Port 3306 : MySQL
```

## 6. Important Learning: `return` vs. `print()`

- `return` sends a value back from a function to the calling code.
- `print()` displays a value on the screen.

Example:

```python
def identify_service(port):
    if port == 22:
        return "SSH"

service = identify_service(22)
print(service)
```

Output:

```text
SSH
```

The function returns `"SSH"`, the variable `service` stores it, and `print()` displays it.

## 7. Limitations

This is a port-classification script, not a network scanner.

It identifies common service associations based on port numbers. It does not check whether a port is open or whether a particular service is actually running on a target.

A penetration tester must verify the actual service using appropriate scanning and investigation techniques.

## 8. Key Takeaways

- A function can receive arguments and return results.
- A loop can repeatedly call a function for each item in a list.
- Conditional statements allow a script to make decisions.
- Variables can store values returned by functions.
- Understanding the execution flow is as important as getting the script to run.

**Status:** Practical script completed and documented.
