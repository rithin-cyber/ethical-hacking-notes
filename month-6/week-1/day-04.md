# Date

2026-07-03

# Objective

Understand the differences between the HTTP GET and POST methods, identify where data is sent, observe both methods in Chrome DevTools and Burp Suite, and understand why each method is used in web applications.

---

# GET

## Purpose

Retrieve data from the server without changing server-side data.

## Advantages

- Parameters are visible in the URL.
- Easy to bookmark and share.
- Can be cached by browsers.
- Useful for searching and viewing information.

## Examples

```http
GET /search?q=laptop HTTP/1.1
```

```http
GET /profile?id=25 HTTP/1.1
```

Real-world examples:

- Google search
- Viewing products
- Viewing profiles
- Reading articles

---

# POST

## Purpose

Send data from the client to the server. Commonly used when data needs to be processed or server state changes.

## Advantages

- Parameters are usually sent in the request body.
- Suitable for sending sensitive or large amounts of data.
- Commonly used for forms and authentication.

## Examples

```http
POST /login HTTP/1.1

username=alice
password=Password123
```

```http
POST /register HTTP/1.1
```

Real-world examples:

- Login
- Registration
- File Upload
- Contact Form
- Password Change

---

# Burp Observations

Captured one GET request and one POST request.

GET Request:

- Parameters appeared in the URL.
- Usually had no request body.
- Used to retrieve information.

Example:

```http
GET /filter?category=Pets HTTP/2
```

POST Request:

- Parameters appeared inside the request body.
- Used to send user input.
- Commonly used for login forms.

Example:

```http
POST /login HTTP/2

username=alice
password=Password123
```

Compared both requests using Burp Repeater without modifying them.

---

# Browser Observations

Used Chrome Developer Tools (F12 → Network).

Observed:

- Search requests generally used GET.
- Login requests used POST.
- GET parameters were visible in the URL.
- POST data was not visible in the URL because it was sent in the request body.
- Refreshing a GET request usually worked normally because the information was contained in the URL.

---

# Interview Notes

## What is GET?

GET is an HTTP method used to retrieve data from a server. Parameters are usually sent in the URL.

---

## What is POST?

POST is an HTTP method used to send data to a server. Parameters are usually sent in the request body.

---

## Difference between GET and POST

| GET | POST |
|------|------|
| Retrieves data | Sends data |
| Parameters in URL | Parameters in request body |
| Usually does not change server data | Often changes server data |
| Can be bookmarked | Not bookmarked in the same way |

---

## Why is POST used for login?

POST sends credentials in the request body instead of placing them in the URL. This avoids exposing usernames and passwords in the address bar, bookmarks, or browser history. Secure transmission still depends on using HTTPS.

---

# What I Learned

- HTTP uses different methods for different purposes.
- GET is mainly used to retrieve information.
- POST is mainly used to send information.
- Parameters are the data sent to the server.
- GET usually places parameters in the URL.
- POST usually places parameters in the request body.
- Chrome DevTools and Burp Suite can both be used to inspect HTTP requests.
- Burp Repeater allows requests to be examined and tested safely.

---

# Questions

## What is a parameter?

A parameter is a piece of data sent from the client to the server.

---

## Where are GET parameters usually located?

In the URL.

Example:

```http
/search?q=laptop
```

---

## Where are POST parameters usually located?

Inside the request body.

Example:

```http
POST /login

username=alice
password=Password123
```

---

## Does every POST request have parameters?

Not always, but POST requests commonly contain parameters in the request body.

---

## What is the request body?

The request body is the section of an HTTP request where data is sent to the server, most commonly with POST requests.

---

## What is the main difference between a parameter and a request body?

A parameter is the data being sent (for example, `username=alice`).

The request body is the location where those parameters are placed in a POST request.
