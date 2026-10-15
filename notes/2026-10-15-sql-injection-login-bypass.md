# SQL Injection — Login Bypass

## Objective
Test whether user input in the login form can alter SQL query behaviour and bypass authentication.

## Environment
- Platform: PortSwigger Web Security Academy
- Lab: SQL injection vulnerability allowing login bypass
- Endpoint: `POST /login`
- Parameter tested: `username`

## Testing
1. Captured the login request in Burp Suite Proxy.
2. Sent the request to Burp Suite Repeater.
3. Modified the username input to test for SQL injection.
4. Submitted the modified request and inspected the server response.

## Evidence
The server returned:

- HTTP status: `302 Found`
- Redirect: `/my-account?id=administrator`

The lab confirmed successful completion.

## Impact
SQL injection allowed authentication to be bypassed, granting access to the administrator account in the training environment. In a real application, similar behaviour could allow unauthorised account access.

## Remediation
- Use parameterised queries or prepared statements.
- Avoid building SQL queries by concatenating user-controlled input.
- Apply appropriate server-side authentication and authorisation checks.
- Handle errors safely without exposing database details.

## Limitations
This test demonstrated the vulnerability in the specified training lab. It does not establish that another application has the same weakness.

## Result
Lab solved successfully. Evidence collected and remediation documented.
