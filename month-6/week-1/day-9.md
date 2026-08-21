# Authentication Security Testing

## Date

2026-08-05

## Objective

Understand how to assess a web application's login mechanism for weaknesses without performing password attacks.

Focus areas:

* Normal authentication behavior
* Authentication failures
* Username enumeration
* Account lockout
* Password policy
* MFA
* Authentication response differences
* Professional documentation

## Normal Login Flow

```text
GET /login
↓
Login page
↓
POST /login
↓
Server verifies credentials
↓
Authentication successful
↓
Session created
↓
Dashboard
```

The first step in authentication testing is to understand the application's normal login behavior.

## Authentication Failure

Example:

```http
POST /login

username=alice
password=wrongpassword
```

Possible response:

```http
HTTP/1.1 401 Unauthorized
```

Authentication failure behavior should be observed and compared with successful authentication behavior.

## Username Enumeration

Username enumeration occurs when an application reveals whether a username exists.

Example:

```text
Existing username
→ "Incorrect password"

Non-existing username
→ "Username does not exist"
```

A tester should compare:

* Status code
* Response body
* Response length
* Error message
* Response time
* Redirect behavior

A difference does not automatically mean a vulnerability. The difference must have security significance.

## Account Lockout

Account lockout limits repeated failed authentication attempts.

Example:

```text
Attempt 1 → Failed
Attempt 2 → Failed
Attempt 3 → Failed
Attempt 4 → Account locked
```

Purpose:

Reduce repeated authentication attempts.

Security consideration:

An overly aggressive lockout can allow someone to intentionally lock other users out of their accounts.

## Password Policy

A secure application may require:

* Minimum password length
* Strong passwords
* Protection against commonly compromised passwords
* Appropriate password reset controls

Password complexity alone does not make authentication secure.

Other important controls include:

* Session management
* MFA
* Password reset security
* Rate limiting
* Access control

## MFA

MFA means **Multi-Factor Authentication**.

Instead of relying only on:

```text
Password
```

the application may require:

```text
Password
+
Authenticator code
```

The purpose is to require more than one authentication factor.

## Burp Observations

### Normal Login Request

```http
POST /login HTTP/1.1
Host: [REDACTED]
Content-Type: application/x-www-form-urlencoded

username=wiener&password=[REDACTED]
```

Observed:

* HTTP Method: `POST`
* Endpoint: `/login`
* Parameters:

  * `username`
  * `password`
* Content-Type: `application/x-www-form-urlencoded`

### Successful Login Response

```http
HTTP/2 302 Found
Location: /login2
Set-Cookie: session=[REDACTED]; Secure; HttpOnly; SameSite=None
Content-Length: 0
```

Observed behavior:

```text
Successful login
↓
302 Found
↓
Redirect to /login2
↓
New session cookie
```

### Existing Username + Wrong Password

```text
Status Code: 200 OK
Response Length: 3137
Error Message: Invalid username or password
Redirect: None
```

### Invalid Username + Wrong Password

```text
Status Code: 200 OK
Response Length: 3137
Error Message: Invalid username or password
Redirect: None
```

### Response Time Comparison

```text
Existing username: 192 ms
Invalid username: 191 ms
```

The 1 ms difference did not provide a meaningful signal by itself.

### Username Enumeration Conclusion

The compared responses were effectively identical:

```text
Status Code       → Same
Response Length   → Same
Error Message     → Same
Redirect          → Same
Response Time     → Essentially same
```

Based on these observations, there was **no obvious username-enumeration signal** in this test.

## Testing Methodology

```text
Normal behavior
↓
Change ONE variable
↓
Observe response
↓
Compare with baseline
↓
Identify differences
↓
Determine security impact
```

Important:

Do not change multiple variables at the same time because it makes the results harder to interpret.

## Interview Questions

### How would you test a web application's login functionality?

A simple interview-quality answer:

> First, I would understand the normal login flow and observe the request and response. Then I would test authentication failure behavior by changing one input at a time and comparing the responses. I would look for username enumeration, rate limiting or account lockout behavior, password policy, MFA, and session security.

### What is username enumeration?

> Username enumeration occurs when an application reveals whether a username exists, for example through different error messages, status codes, response lengths, response times, or redirects.

### Why compare responses?

> Comparing responses helps determine whether changing one input produces meaningful differences in application behavior.

## What I Learned

* Authentication testing starts with understanding normal login behavior.
* A successful login and a failed login can produce different HTTP responses.
* Username enumeration can occur when the application reveals whether an account exists.
* Status code, response length, error message, redirect, and response time can all be useful comparison points.
* The tested application returned the same observable behavior for an existing username with a wrong password and a clearly invalid username with a wrong password.
* No obvious username-enumeration signal was identified from those observations.
* Account lockout can protect against repeated authentication attempts but can also introduce denial-of-service concerns if designed poorly.
* Password complexity alone is not enough to secure authentication.
* MFA adds an additional authentication factor.
* A professional tester should change one variable at a time and compare the result with a baseline.
