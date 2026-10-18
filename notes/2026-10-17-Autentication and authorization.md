# Day 12 — Authentication Security Testing

## 1. Authentication vs Authorization

- **Authentication:** Verifies who a user is.
- **Authorization:** Determines what an authenticated user is allowed to access.

## 2. Common Authentication Weaknesses

### Username Enumeration
Occurs when an application reveals whether a username exists through different login responses.

**Prevention:** Use consistent error messages, such as `Invalid username or password`.

### Brute-Force Protection Failure
Occurs when an application does not adequately restrict repeated login attempts.

**Prevention:** Apply rate limiting and appropriate protections against automated password guessing.

### Weak Passwords
Common or easily guessed passwords make accounts easier to compromise.

**Prevention:** Block commonly used passwords and enforce appropriate password policies.

### Password Reset Flaws
A password reset process may be vulnerable if reset tokens can be reused after successful use.

**Prevention:** Invalidate reset tokens after use and implement secure reset procedures.

## 3. PortSwigger Practical Lab

**Lab:** Username enumeration via different responses

**Status:** Solved independently.

**What I learned:** Different error messages can reveal valid usernames. A valid username paired with an incorrect password may return a different response from an invalid username.

**Security impact:** Attackers may identify valid accounts and use that information to support further attacks.

**Remediation:** Use consistent authentication responses and avoid revealing whether an account exists.

## 4. Login Rate Limiting

Rate limiting restricts repeated login attempts and helps reduce brute-force attacks and automated password guessing.

## 5. Key Takeaways

- Authentication verifies identity; authorization controls access.
- Different login responses can enable username enumeration.
- Rate limiting helps reduce brute-force attacks.
- Password reset tokens should not remain valid after successful use.
- Security findings should include the vulnerability, evidence, impact and remediation.

## 6. Practice Record

- Completed authentication recall questions.
- Reviewed common authentication weaknesses.
- Independently solved the PortSwigger username-enumeration lab.
- Practised explaining vulnerabilities and their mitigations.
