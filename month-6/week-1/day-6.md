# Date

2026-07-05

# Objective

Understand the HTTP methods beyond GET and POST, learn when they are used, recognize them in Burp Suite, and understand why penetration testers pay attention to them.

---

# HTTP Methods

## GET

**Purpose:**
Retrieve (read) data from the server.

**Example:**
Viewing a Facebook profile or an Amazon product page.

---

## POST

**Purpose:**
Send data to the server.

**Example:**
Submitting login credentials, registering an account, or uploading a file.

---

## PUT

**Purpose:**
Replace an entire resource.

**Example:**
Updating all profile information, such as name, email, phone number, and address.

---

## PATCH

**Purpose:**
Update part of a resource.

**Example:**
Changing only the email address without modifying the rest of the profile.

---

## DELETE

**Purpose:**
Delete a resource from the server.

**Example:**
Deleting a user account or removing a file.

---

## OPTIONS

**Purpose:**
Ask the server which HTTP methods are allowed for a resource.

**Example:**
Used by browsers during CORS preflight requests.

---

## HEAD

**Purpose:**
Retrieve only the response headers without downloading the response body.

**Example:**
Checking whether a file exists, its size, or the last modified date before downloading it.

---

# HTTP Methods Comparison

| Method | Purpose |
|---------|---------|
| GET | Retrieve data |
| POST | Send/Create data |
| PUT | Replace an entire resource |
| PATCH | Partially update a resource |
| DELETE | Delete a resource |
| OPTIONS | Discover allowed HTTP methods |
| HEAD | Retrieve headers only |

---

# Burp Observations

- Captured a GET request from PortSwigger.
- Changed the request method from GET to HEAD in Burp Repeater.
- The server responded with:

```
HTTP/2 404 Not Found
```

Observed response headers:

- Content-Type
- X-Frame-Options
- Content-Length

The server returned **404 Not Found** because the endpoint did not support the HEAD request (or the resource was unavailable for that method).

---

# Browser Observations

- Most normal browsing generated GET and POST requests.
- PUT, PATCH, DELETE, OPTIONS, and HEAD were not commonly observed while browsing.
- These methods are more common in REST APIs than in normal webpage navigation.

---

# Pentester Notes

Penetration testers pay attention to HTTP methods because they determine what actions can be performed on a resource.

Examples:

- Test whether an unauthorized user can send a DELETE request.
- Check whether a hidden PUT endpoint allows modifying data.
- Use OPTIONS to discover supported HTTP methods.
- Test whether PATCH can update sensitive fields such as `role` or `isAdmin`.

---

# Interview Notes

- GET retrieves data.
- POST sends data.
- PUT replaces an entire resource.
- PATCH updates part of a resource.
- DELETE removes a resource.
- OPTIONS tells which HTTP methods are allowed.
- HEAD returns only the response headers.

Difference between PUT and PATCH:

- PUT replaces the complete resource.
- PATCH updates only specific fields.

---

# What I Learned

- Learned five additional HTTP methods beyond GET and POST.
- Learned when each method is commonly used.
- Practiced changing GET to HEAD in Burp Repeater.
- Understood why penetration testers test PUT, PATCH, DELETE, and OPTIONS endpoints.
- Learned that HTTP methods are important for authorization testing and REST API security.

---

# Questions

- Why do some servers return 404 when changing GET to HEAD?
- Why do some APIs allow PUT while others use PATCH?
- How do penetration testers discover hidden HTTP methods during an assessment?
