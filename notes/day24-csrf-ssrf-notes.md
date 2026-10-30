# CSRF and SSRF — Day 24 Notes

**Date:** 2026-10-30  
**Study track:** Technical interview preparation  
**Level:** Beginner

## 1. Cross-Site Request Forgery (CSRF)

### Full form
**CSRF** = **Cross-Site Request Forgery**

### Simple meaning
CSRF is a web security vulnerability in which an attacker tricks a user's browser into sending an unwanted request to a website where the user is already authenticated.

The browser may automatically include the website's session cookies with the request. If the website does not properly protect state-changing actions, the unwanted action may succeed.

### Example
Imagine you are logged in to your bank:

1. You sign in to your bank account.
2. In another tab, you visit a malicious website.
3. That website causes your browser to send an unwanted request to the bank.
4. Your browser may include your bank's authentication cookie.
5. If the bank has no effective CSRF protection, the request could perform an action you did not intend, such as changing account details or initiating a transfer.

**Important:** A CSRF attack does not automatically work on every logged-in website. It depends on the application's authentication method, browser cookie behavior, request design, and whether effective CSRF protections are in place.

### What is being tricked?
The **user's browser** is tricked into sending a request to a site where the user is authenticated.

### Common protections
- **CSRF tokens:** The application requires an unpredictable, session-associated token with state-changing requests.
- **SameSite cookies:** Configure session cookies appropriately (`Lax` or `Strict` where compatible) to reduce cross-site cookie sending.
- **Origin/Referer validation:** Validate the request's origin for sensitive state-changing operations.
- **Re-authentication or confirmation:** Require an additional check for especially sensitive actions.

### What a penetration tester should check
Only test applications where you have permission.

- Identify state-changing actions, such as changing an email address or password.
- Check whether the application uses an anti-CSRF token.
- Check whether the token is validated on the server and cannot simply be omitted or reused in an invalid context.
- Review the session cookie's `SameSite` attribute.
- Check whether the server validates `Origin` or `Referer` where appropriate.
- Document the affected action, prerequisites, impact, and evidence without performing unauthorized actions.

### Interview answer
> CSRF stands for Cross-Site Request Forgery. It happens when an attacker tricks an authenticated user's browser into sending an unwanted request to a website. It can be prevented with CSRF tokens, appropriate SameSite cookie settings, and origin validation.

### Quick revision
- **CSRF:** Cross-Site Request Forgery.
- **What gets tricked?** The user's browser.
- **What is abused?** The user's authenticated session and the browser's request behavior.
- **Main protections:** CSRF tokens, SameSite cookies, and origin validation.

---

## 2. Server-Side Request Forgery (SSRF)

### Full form
**SSRF** = **Server-Side Request Forgery**

### Simple meaning
SSRF is a web security vulnerability in which an attacker manipulates an application so that the application's **server** makes a request to an unintended destination.

### Example
Imagine a website has a feature that fetches an image from a URL supplied by a user:

1. The user provides an image URL.
2. The website's server retrieves the image from that URL.
3. An attacker may try to supply a URL that points to an internal service instead of a normal public image.
4. If the server accepts and fetches that destination, it may reach internal resources that should not be exposed to the user.

Depending on the application and its network access, SSRF may expose internal services or cloud metadata endpoints. Some SSRF issues are **blind**: the server makes the request, but the response is not shown directly to the user.

### What is being tricked?
The **application server** is tricked into making a request to an unintended destination.

### Common protections
- **Destination allowlisting:** Permit requests only to approved hosts and destinations when possible.
- **Validate destinations:** Check URL schemes, hostnames, resolved IP addresses, and ports.
- **Block sensitive address ranges:** Prevent access to loopback, private, link-local, and other restricted internal ranges unless explicitly required.
- **Control redirects and DNS resolution:** Re-check destinations after redirects and guard against DNS rebinding or hostname-to-IP changes.
- **Network egress controls:** Restrict which systems the application server can contact.
- **Avoid unnecessary URL fetching:** Remove or redesign server-side fetch features that are not needed.

### What a penetration tester should check
Only test systems where you have explicit permission and an agreed scope.

- Identify features that make the server fetch a URL, such as image importers, URL previews, webhooks, or PDF generators.
- Understand which URL schemes, hostnames, and ports the feature accepts.
- In an authorized test environment, use a tester-controlled endpoint to confirm whether the server makes an outbound request.
- Check whether redirects or DNS resolution can bypass destination restrictions.
- Determine whether the behavior is blind or whether any fetched response is returned.
- Record the affected feature, the server-side request evidence, impact, and recommended controls.

### Interview answer
> SSRF stands for Server-Side Request Forgery. It happens when an attacker tricks an application server into making requests to unintended internal or external resources. It can be reduced by validating and allowlisting destinations, blocking access to sensitive internal ranges, and restricting outbound network connections.

### Quick revision
- **SSRF:** Server-Side Request Forgery.
- **What gets tricked?** The application server.
- **What is abused?** A server-side feature that fetches a URL or makes a network request.
- **Main protections:** Destination allowlisting, destination validation, and network egress restrictions.

---

## 3. CSRF vs. SSRF

| Point | CSRF | SSRF |
|---|---|---|
| Full form | Cross-Site Request Forgery | Server-Side Request Forgery |
| What is tricked? | The user's browser | The application server |
| Who sends the unwanted request? | The victim's browser | The vulnerable server |
| Typical example | An unwanted action using a logged-in user's browser session | A server fetches an attacker-influenced URL pointing to an unintended destination |
| Main protections | CSRF tokens, SameSite cookies, origin validation | Destination allowlisting and validation, egress controls |

**Memory trick:**
- **CSRF = browser sends an unwanted request.**
- **SSRF = server sends an unwanted request.**

---

## 4. Important interview checks

1. **What does CSRF stand for?**  
   Cross-Site Request Forgery.

2. **In CSRF, what gets tricked?**  
   The user's browser.

3. **What does SSRF stand for?**  
   Server-Side Request Forgery.

4. **In SSRF, what gets tricked?**  
   The application server.

5. **Give one CSRF protection.**  
   Use and validate anti-CSRF tokens. SameSite cookies and origin validation can provide additional protection.

6. **Give one SSRF protection.**  
   Allowlist permitted destinations and restrict the server's outbound network access.

---

## 5. Topics to revise next

- Continue the remaining OWASP-related vulnerability topics.
- Review the difference between CSRF and SSRF until it can be explained without notes.
- Practice giving each interview answer in two or three clear sentences.

**Study status:** CSRF and SSRF explanations documented. Day 24 is not yet complete.
