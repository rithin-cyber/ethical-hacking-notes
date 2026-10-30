# Day 24 — Completion Summary

**Date:** 30 October 2026  
**Status:** COMPLETE — closed as requested by the learner  
**Next day:** Day 25 — cybersecurity job search

## 1. Day 24 outcome

Day 24 focused on technical interview preparation and explaining cybersecurity concepts clearly in a short recruiter/interview answer.

The planned technical interview practice session is marked complete. The learner practised answering questions aloud, received corrections, and refined several answers into shorter interview-ready responses.

This is a completion record for the Day 24 study session, not a claim that every cybersecurity topic has been mastered. Topics that need more revision are listed below and can be revisited during later study sessions.

## 2. Topics practised

### Interview and security fundamentals

- Personal introduction for an entry-level penetration-testing role
- What penetration testing is and why authorization matters
- Engagement planning and scope before technical testing
- Nmap: host discovery, open ports, services, and versions
- HTTP service discovery on port 80 and basic web reconnaissance
- Investigating an outdated Apache version: confirm the version, check trustworthy advisories/CVEs, then validate safely within scope
- Vulnerability, exploit, and payload
- TCP versus UDP, including acknowledgements and retransmission
- Symmetric and asymmetric encryption
- Authentication versus authorization
- Two-factor authentication and common weaknesses
- Session management and session tokens
- IDOR and horizontal privilege escalation
- SQL injection and parameterized queries/prepared statements
- Username enumeration and consistent login error messages
- Threat modeling

### Web vulnerability concepts practised

- SQL Injection
- Cross-Site Scripting (XSS)
- Cross-Site Request Forgery (CSRF)
- Server-Side Request Forgery (SSRF)
- Insecure Direct Object Reference (IDOR)
- Authentication and authorization failures
- Session management weaknesses
- Security misconfiguration
- Insecure design
- Software supply chain failures
- Cryptographic failures

## 3. OWASP Top 10:2025 — revision record

The learner practised recalling all ten category names and explaining examples. The categories are:

1. **A01 — Broken Access Control**
2. **A02 — Security Misconfiguration**
3. **A03 — Software Supply Chain Failures**
4. **A04 — Cryptographic Failures**
5. **A05 — Injection**
6. **A06 — Insecure Design**
7. **A07 — Authentication Failures**
8. **A08 — Software or Data Integrity Failures**
9. **A09 — Security Logging and Alerting Failures**
10. **A10 — Mishandling of Exceptional Conditions**

Important mapping examples discussed:

- IDOR → A01 Broken Access Control
- Default credentials / insecure directory settings → A02 Security Misconfiguration
- Compromised third-party library → A03 Software Supply Chain Failures
- Weak or missing protection for sensitive data → A04 Cryptographic Failures
- SQL Injection and OS Command Injection → A05 Injection
- CSRF was discussed as a web vulnerability topic; do not use it as an example of A03.
- SSRF is included under A01 in the OWASP Top 10:2025 classification.

The learner practised all ten names and basic meanings. More detailed prevention-method recall, especially for A04–A10, remains a useful future revision item; it does not keep this Day 24 session open.

## 4. Corrected short interview answers

### Broken Access Control

> Broken Access Control happens when a user can access data or perform actions they are not authorized to access or perform. For example, changing an ID in a URL to see another user's data. Prevent it with server-side authorization checks on every request.

### Security Misconfiguration

> Security Misconfiguration means an application or server has insecure settings, such as default administrator credentials. Prevent it by removing defaults and applying secure configurations.

### Software Supply Chain Failures

> Software Supply Chain Failures happen when compromised or untrusted software components, such as a third-party library, affect an application. Reduce the risk by using trusted dependencies, keeping them updated, and verifying their integrity.

### Cryptographic Failures

> Cryptographic Failures happen when sensitive data is not properly protected, such as when weak or no encryption is used. Protect data with suitable modern cryptography and secure key management.

### Injection

> Injection happens when an application treats untrusted user input as a command or query. For SQL Injection, use parameterized queries or prepared statements.

### CSRF versus SSRF

- **CSRF:** tricks a user's browser into sending an unwanted request using the user's authenticated session.
- **SSRF:** tricks an application server into making requests to unintended destinations.

## 5. Weak-answer and revision log

These are the main points that needed correction or extra practice during the session:

| Topic | Issue noticed | Correct idea to remember |
|---|---|---|
| Broken Access Control | Initially described it as improper input handling | It is an authorization/access-control failure. Enforce checks on the server. |
| Security Misconfiguration | Mixed it up with SQL Injection | It means insecure settings, such as default credentials or exposed directory listing. |
| Software Supply Chain | The example was initially incomplete | Say “a compromised third-party library” or “compromised dependency.” Verify integrity. |
| Cryptographic Failures | Initially focused only on missing HTTPS | It also includes weak cryptography, exposed/poorly managed keys, and unsafe data storage/transmission. |
| Injection | Needed help stating the definition fluently | “The application treats untrusted input as a command or query.” |
| 2FA bypass | Suggested that displaying a code itself was a bypass | Examples include phishing, SIM swapping, and weak account-recovery flows. |
| Session management | Definition was vague | Sessions let the server associate requests with a logged-in user; protect session tokens and cookies. |
| TCP and UDP | Needed help describing packet delivery | TCP uses acknowledgements and retransmission; UDP does not guarantee delivery. |
| Apache version checking | Jumped from finding a version to using Metasploit | Confirm the exact version, review vendor advisories/CVEs, and validate safely in scope. An exploit listing alone does not prove the target is vulnerable. |
| Threat modeling | Needed prompting to recall the term | Identify possible threats and abuse cases before/during design. |
| CSRF / SSRF | Confused which side is tricked | CSRF abuses a user's browser/session; SSRF makes the server send requests. |

## 6. Speaking practice method

For technical interview answers, use this short structure:

1. **Meaning:** one sentence.
2. **Example:** one realistic example.
3. **Prevention:** one practical control.

Avoid long paragraphs. Pause between ideas instead of repeating words or filling silence with “uh.” The objective is a correct, understandable answer, not a memorized speech.

## 7. Documentation created during Day 24

- `day24-csrf-ssrf-notes.md` — focused CSRF and SSRF study notes.
- `day24-owasp-interview-practice-progress.md` — OWASP interview practice progress and examples for A01–A03, with A04 introduced.
- This file — consolidated Day 24 completion summary and weak-answer log.

## 8. Day 24 completion decision

**Day 24 is marked COMPLETE at the learner's request.** The technical interview preparation session and speaking practice for this day are closed. The notes and weak-answer log are recorded.

Remaining OWASP prevention details and additional interview questions can be revisited as normal future revision; they are not being carried as unfinished Day 24 tasks.

## 9. Day 25 plan — Cybersecurity job search

**Primary goal:** Find and evaluate 3–5 genuinely suitable cybersecurity roles in India.

Priorities:

- Paid positions; remote from India preferred.
- Junior penetration testing, VAPT, application security, offensive security, and security research.
- Include suitable internships when paid and genuinely practical.
- Respect the learner's Higher Secondary Computer Science background, eJPT certification, lab-based experience, and GitHub portfolio.
- Do not recommend roles with mandatory degree requirements the learner does not meet.
- Do not repeat employers already applied to, closed roles, or roles previously rejected as unsuitable unless the learner asks to revisit them.
- Verify the employer, vacancy status, location/work arrangement, experience, education requirements, compensation/stipend, and official application method before recommending an application.

### Companies already in the application/review tracker

Use the existing application tracker as the source of truth before recommending any employer again. Known exclusions include companies and roles previously marked as applied, closed, bounced, unsuitable, or already reviewed, such as PoCForge, HacFy, Stravorus, Fam, Infoziant, Secure Blink, Ziroday, Secure Cyber Future, Thoropass, and other entries already recorded in the user's tracker.

**Day 25 completion target:** 3–5 screened job opportunities, with a clear Apply / Skip decision and the exact official application link for each Apply recommendation.
