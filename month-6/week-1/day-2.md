# Date

2026-07-02

# Objective

Understand how HTTP request and response headers work, what information they contain, and why they are important for web application security and penetration testing.

---

# Request Headers

### Host

**Value:**
```
example.com
```

**Purpose:**
Specifies the website (host) the browser wants to communicate with. It allows the server to determine which website should handle the request.

---

### User-Agent

**Value:**
```
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36
```

**Purpose:**
Identifies the browser, operating system, and device making the request.

---

### Cookie

**Value:**
```
No Cookie header was present in this request.
```

**Purpose:**
Stores small pieces of data (such as a Session ID) in the browser and automatically sends them to the server on future requests to maintain the user's session.

---

### Referer

**Value:**
```
No Referer header was present in this request.
```

**Purpose:**
Indicates the webpage from which the current request originated.

---

### Authorization

**Value:**
```
No Authorization header was present in this request.
```

**Purpose:**
Carries authentication credentials (such as Basic Authentication or Bearer Tokens) to verify the user's identity.

---

# Response Headers

### Set-Cookie

**Value:**
```
No Set-Cookie header was present in the response.
```

**Purpose:**
Sent by the server to instruct the browser to create and store a cookie.

---

### Content-Type

**Value:**
```
Content-Type: text/html
```

**Purpose:**
Tells the browser what type of content is being returned by the server.

---

### Location

**Value:**
```
No Location header was present.
```

**Purpose:**
Used during redirects to tell the browser where it should go next.

---

### Server

**Value:**
```
Server: cloudflare
```

**Purpose:**
Identifies the server software or service handling the request.

---

# Browser Observation

Observed the HTTP request and response using Chrome Developer Tools (F12 → Network → Headers).

Learned how to identify:
- Request Headers
- Response Headers
- Status Code
- Request Method
- Request URL

---

# Burp Observation

Repeated observation can be performed in Burp Suite by:

- Proxy → HTTP History
- Select the request
- View:
  - Request Headers
  - Response Headers
  - Cookies
  - Status Code
  - HTTP Method

Burp provides the same HTTP information as the browser but also allows interception and modification of requests.

---

# Questions

### What is the purpose of the Host header?

It tells the server which website the browser wants to access.

---

### What is the purpose of the User-Agent header?

It identifies the browser, operating system, and device making the request.

---

### What is the purpose of the Cookie header?

It sends stored cookies (such as a Session ID) from the browser to the server.

---

### What is the purpose of the Set-Cookie header?

It tells the browser to create and store a cookie.

---

### What does the Content-Type header tell us?

It tells the browser what type of data the server returned (HTML, JSON, image, etc.).

---

### Why is the Server header useful?

It provides information about the server software or service handling the request, which may help during reconnaissance.

---

# What I Learned

- HTTP communication consists of requests and responses.
- Request headers provide information from the browser to the server.
- Response headers provide information from the server to the browser.
- The **Host** header specifies the destination website.
- The **User-Agent** header identifies the browser and operating system.
- The **Cookie** header sends stored cookies to maintain sessions.
- The **Set-Cookie** header creates cookies in the browser.
- The **Content-Type** header specifies the type of data returned.
- The **Server** header identifies the server software or service.
- Chrome Developer Tools and Burp Suite can both be used to inspect HTTP requests and responses.
