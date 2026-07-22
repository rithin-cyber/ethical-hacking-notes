# Date

2026-07-04

# Objective

Understand HTTP responses, HTTP status codes, and why status codes are important during web application penetration testing.

---

# Response Structure

## 1. Status Line

Contains:

- HTTP Version
- Status Code
- Reason Phrase

Example:

```http
HTTP/2 200 OK
```

Example:

- HTTP Version: HTTP/2
- Status Code: 200
- Reason Phrase: OK

---

## 2. Response Headers

Provide additional information about the server's response.

Example:

```http
Content-Type: text/html
Server: cloudflare
Last-Modified: Mon, 20 Jul 2026 07:16:20 GMT
Allow: GET, HEAD
```

Common Response Headers:

- Content-Type – Type of content returned by the server.
- Server – Server software or service handling the request.
- Set-Cookie – Creates or updates cookies.
- Last-Modified – Last modification time of the resource.
- Location – Redirect destination.
- Allow – Supported HTTP methods.

---

## 3. Response Body

Contains the actual content returned by the server.

Example:

```html
<html>
Welcome back
</html>
```

---

# HTTP Status Code Categories

## 1xx – Informational

Meaning:

The request has been received and processing continues.

Examples:

- 100 Continue
- 101 Switching Protocols

---

## 2xx – Success

Meaning:

The request was successfully processed.

Examples:

- 200 OK
- 201 Created
- 204 No Content

---

## 3xx – Redirection

Meaning:

The browser should request another location or use cached content.

Examples:

- 301 Moved Permanently
- 302 Found
- 304 Not Modified
- 307 Temporary Redirect

---

## 4xx – Client Errors

Meaning:

There is a problem with the client's request.

Examples:

- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 405 Method Not Allowed

---

## 5xx – Server Errors

Meaning:

The server encountered an unexpected error while processing the request.

Examples:

- 500 Internal Server Error
- 502 Bad Gateway
- 503 Service Unavailable

---

# Browser Observations

Website:

https://example.com

Observed Status Codes:

- 304 Not Modified
- Browser used the cached version of the page because it had not changed.

Website:

https://kome.ai/api/user/status

Observed Status Code:

- 200 OK
- The server successfully processed the request.

Website:

https://example.com/thispagedoesnotexist

Observed Status Code:

- 404 Not Found
- The requested resource did not exist on the server.

---

# Burp Observations

Response:

```http
HTTP/2 200 OK
Content-Type: text/html
Server: cloudflare
```

Observed:

- HTTP Version: HTTP/2
- Status Code: 200 OK
- Content-Type: text/html
- Server: cloudflare
- Allow Header: GET, HEAD
- Last-Modified header present
- Cloudflare cache headers were present.

Sent the request to Burp Repeater.

Result:

- Status code remained 200 OK because the request was not modified.

---

# Interview Notes

HTTP Response consists of:

1. Status Line
2. Response Headers
3. Response Body

Common Status Codes:

- 200 OK – Request processed successfully.
- 301 Moved Permanently – Resource has permanently moved.
- 302 Found – Temporary redirect.
- 304 Not Modified – Browser should use its cached copy.
- 401 Unauthorized – Authentication is required or failed.
- 403 Forbidden – Access denied even if authenticated.
- 404 Not Found – Requested resource does not exist.
- 500 Internal Server Error – Unexpected server-side error.

Reason Phrase:

Human-readable text that follows the status code.

Example:

```
HTTP/2 404 Not Found
```

Status Code:

404

Reason Phrase:

Not Found

Why Status Codes Matter in Penetration Testing:

- Show how the application responds to requests.
- Help identify authentication and authorization issues.
- Help detect redirects.
- Help identify missing resources.
- Help identify server errors after sending test payloads.

---

# What I Learned

- Every HTTP response contains a Status Line, Response Headers, and a Response Body.
- Status codes are divided into five categories (1xx–5xx).
- 200 indicates success.
- 304 indicates cached content should be used.
- 401 means authentication failed or is required.
- 403 means access is denied.
- 404 means the resource does not exist.
- 500 indicates a server-side error.
- Burp Repeater allows requests to be resent and modified while observing server responses.

---

# Questions

- Why do some websites return 200 instead of 404 for missing pages?
- What information can be learned from different response headers?
- How can changing a request in Burp Repeater affect the response status code?
