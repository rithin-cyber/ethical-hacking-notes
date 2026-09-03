# Authentication Testing Methodology

## Objective

Understand and practice a systematic methodology for testing a web application's authentication mechanism.

The methodology used today:

```text
Understand
↓
Establish baseline
↓
Identify inputs
↓
Change ONE variable
↓
Compare responses
↓
Determine security impact
↓
Document evidence
↓
Recommend remediation
```

## Normal Authentication Flow

```text
GET /login
↓
Login form
↓
POST /login
↓
Credential verification
↓
Session creation
↓
Redirect
↓
Authenticated area
```

For a normal login, record:

* Login URL
* HTTP method
* Parameters
* Content-Type
* Status code
* Redirect
* `Set-Cookie`
* Session cookie name

## Baseline

A baseline means understanding what normal authentication behavior looks like before testing changes.

Observed normal login:

```text
Correct username + correct password
↓
302 Found
↓
Location: /login2
↓
Session created / updated
↓
Authenticated area
```

Observed response:

```http
HTTP/2 302 Found
Location: /login2
Set-Cookie: session=[REDACTED]; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

Baseline observations:

```text
Login result: Successful
Status: 302 Found
Session cookie: session=[REDACTED]
Redirect: /login2
Authenticated page: /login2
```

## Authentication Failure

Tested:

```text
Known username + wrong password
```

Observed:

```text
Status: 200 OK
Error: Invalid username or password.
Response length: 3137
Redirect: None
```

This provides a baseline for failed authentication behavior.

## Username Enumeration

Username enumeration occurs when an application reveals whether a username exists.

Examples of differences that may reveal usernames:

```text
Valid username
→ Incorrect password

Invalid username
→ Username does not exist
```

Other useful comparison points:

* Status code
* Response body
* Response length
* Error message
* Redirect behavior
* Response time

Test performed:

```text
Known username + wrong password
↓
Invalid username + wrong password
```

Observed results:

```text
Known username:
Status: 200 OK
Response length: 3137
Error: Invalid username or password.
Redirect: None
Response time: 210 ms

Invalid username:
Status: 200 OK
Response length: 3137
Error: Invalid username or password.
Redirect: None
Response time: 189 ms
```

Conclusion:

> The responses appear consistent overall. Although the response times differed by 21 ms, this difference alone does not reliably demonstrate username enumeration.

A difference does not automatically mean a vulnerability. The difference must reliably reveal whether an account exists.

## Rate Limiting

Rate limiting controls how frequently authentication attempts can be made.

Example:

```text
Attempt 1 → allowed
Attempt 2 → allowed
Attempt 3 → allowed
...
Too many attempts → temporarily restricted
```

During authorized testing, determine whether reasonable protection exists.

Do not perform high-volume credential attacks.

## Account Lockout

Account lockout can temporarily block further authentication attempts after repeated failures.

Example:

```text
Failed attempts
↓
Account temporarily locked
```

Purpose:

Help defend against repeated password-guessing attempts.

Security consideration:

> An overly aggressive lockout can itself become a denial-of-service problem because an attacker could intentionally cause legitimate users' accounts to become locked.

## Password Reset

At a high level, review:

* Is the reset process authenticated appropriately?
* Is the reset token unpredictable?
* Does the token expire?
* Can the token be reused?
* Does the old password remain valid after reset?
* Are reset links protected?

These are questions to understand before deeper password-reset testing.

## Session Security

After successful authentication, check:

```text
Session ID
↓
Does it change after login?
↓
Are cookie attributes appropriate?
↓
Does logout invalidate the session?
↓
Does the session expire?
```

Important:

> A new Session ID after authentication helps prevent session fixation.

## Burp Observations

### Login Request

```http
POST /login HTTP/2
Host: [REDACTED]
Cookie: session=[REDACTED]
Content-Type: application/x-www-form-urlencoded

username=wiener&password=[REDACTED]
```

Observed:

```text
Method: POST
Endpoint: /login
Parameter: username
Parameter: password
Content-Type: application/x-www-form-urlencoded
Cookie name: session
```

### Successful Login Response

```http
HTTP/2 302 Found
Location: /login2
Set-Cookie: session=[REDACTED]; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

Interpretation:

```text
302
↓
Browser is redirected to /login2

Set-Cookie
↓
Browser stores the Session ID in a cookie
```

### Failed Login Response

```http
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
Set-Cookie: session=[REDACTED]; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 3137
```

Error:

```text
Invalid username or password.
```

### Invalid Username Response

```http
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
Set-Cookie: session=[REDACTED]; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 3137
```

Error:

```text
Invalid username or password.
```

## Findings

No obvious username-enumeration vulnerability was identified from the tested response characteristics.

The following were consistent:

```text
Status code
Response length
Error message
Redirect behavior
```

The response-time difference was:

```text
210 ms vs 189 ms
```

A 21 ms difference by itself is not enough to establish a reliable vulnerability.

## Recommendations

Authentication systems should:

* Avoid revealing whether a username exists.
* Use consistent authentication error messages.
* Implement appropriate rate limiting.
* Design account lockout carefully to avoid denial-of-service abuse.
* Protect password-reset functionality.
* Regenerate the Session ID after successful authentication.
* Properly secure and invalidate sessions.

## What I Learned

* A professional pentester does not randomly test authentication.
* First understand the normal login process.
* Establish a baseline before changing inputs.
* Change one variable at a time.
* Compare authentication responses.
* Status code, response length, error message, redirects, and response time can reveal differences.
* A difference does not automatically mean a vulnerability.
* Username enumeration occurs when an application reveals whether an account exists.
* Rate limiting controls how frequently authentication attempts can be made.
* Excessive account lockout can create a denial-of-service problem.
* Password-reset security must also be assessed.
* Session security is an important part of authentication testing.
* Burp Suite can be used to inspect and compare authentication requests and responses.
* A request is a message sent by the client to the server.
* A response is the server's reply to the client.

## Questions

### What is a baseline?

A baseline is the normal behavior of the application that is established before testing changes.

### Why establish a baseline before changing authentication inputs?

It gives the tester something to compare against and helps identify meaningful changes caused by the test.

### What should be compared when testing login behavior?

Status code, response length, error message, redirect behavior, and response time.

### What is rate limiting?

Rate limiting controls how frequently authentication attempts can be made.

### Why is account lockout useful?

It can limit repeated failed authentication attempts and help defend against password guessing.

### How can account lockout be abused?

An attacker may intentionally trigger account lockouts and prevent legitimate users from logging in.

### What should be considered when assessing password reset?

The tester should consider token unpredictability, expiration, reuse, authentication requirements, old-password validity, and link protection.

### Why should a Session ID change after authentication?

Generating a new Session ID after authentication helps prevent session fixation.

### What is the difference between a security difference and a vulnerability?

A security difference is simply a difference in application behavior. It becomes a vulnerability when the difference has a meaningful and exploitable security impact.

### What evidence should a penetration tester document for an authentication finding?

The tester should document the finding, impact, evidence, steps to reproduce, and remediation.

## Testing Methodology

```text
Normal behavior
↓
Establish baseline
↓
Change ONE variable
↓
Observe response
↓
Compare with baseline
↓
Determine security impact
↓
Document evidence
↓
Recommend remediation
```
