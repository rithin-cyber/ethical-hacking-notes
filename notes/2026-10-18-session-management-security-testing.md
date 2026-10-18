# Day 13 — Session Management Security Testing

## 1. Session Fixation vs Session Hijacking

- **Session fixation:** An attacker makes a victim use a session ID the attacker already knows.
- **Session hijacking:** An attacker obtains a valid session ID and uses it to impersonate the victim.

## 2. Important Cookie Attributes

- **HttpOnly:** Prevents JavaScript from reading the cookie.
- **Secure:** Allows the browser to send the cookie only over HTTPS.
- **SameSite:** Controls when cookies are sent with cross-site requests and helps reduce CSRF risk.

## 3. Secure Session Management

- Regenerate the session ID after successful login.
- Invalidate the session on the server when the user logs out.
- Use cryptographically random, unpredictable persistent-login tokens.
- Apply appropriate expiry and revocation controls.
- Protect session cookies with suitable security attributes.

## 4. PortSwigger Practical Lab

**Lab:** Brute-forcing a stay-logged-in cookie

**Status:** Completed with solution guidance.

**Key learning:** Base64 is encoding, not encryption. A predictable cookie containing a username and a weak MD5 password hash can expose an account to offline password guessing.

**Remediation:** Use secure password-hashing algorithms and random, unpredictable persistent-login tokens validated securely by the server.

## 5. Key Takeaways

- Regenerate session IDs after authentication.
- Invalidate sessions on logout.
- Protect session cookies with appropriate attributes.
- Never treat Base64 encoding as encryption.
- Use secure token generation, expiry and revocation.
- Document vulnerabilities with evidence, impact and remediation.

## 6. Practice Record

- Completed five initial recall questions.
- Reviewed session-management security concepts.
- Completed the PortSwigger lab with solution guidance.
- Practised explaining findings and mitigations.
