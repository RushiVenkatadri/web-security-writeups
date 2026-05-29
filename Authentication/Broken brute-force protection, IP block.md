Broken Brute-Force Protection, IP Block

Overview

This lab demonstrates how brute-force protection mechanisms can be bypassed despite the application attempting to restrict repeated authentication attempts.

The application blocks login attempts after detecting excessive failed requests from a single source. However, weaknesses in the implementation allow attackers to continue testing credentials and eventually compromise user accounts.

This vulnerability is a form of Broken Authentication.

---

Vulnerability Description

Many applications attempt to defend against brute-force attacks by:

- Blocking IP addresses
- Limiting login attempts
- Locking user accounts
- Introducing delays between requests

If these protections are not implemented correctly, attackers may discover methods to bypass them and continue automated credential attacks.

---

Lab Objective

Bypass the application's brute-force protection and gain access to Carlos's account.

---

Steps Performed

1. Analyze Login Functionality

Intercepted login requests using Burp Suite.

Observed a standard authentication request:

POST /login HTTP/1.1

username=carlos
password=test123

---

2. Trigger Brute-Force Protection

Performed multiple failed login attempts.

After several requests, the application activated brute-force protection and began restricting authentication attempts.

---

3. Study Protection Mechanism

Analyzed how the application tracked authentication failures.

Observed that protection logic relied on predictable criteria that could be manipulated during testing.

---

4. Bypass Login Restrictions

Modified the attack strategy to avoid triggering the application's protection mechanism.

Continued testing credentials while remaining below detection thresholds.

---

5. Discover Valid Credentials

Eventually identified a valid password for the target account.

The application's defenses failed to prevent continued password guessing.

---

6. Access Carlos's Account

Authenticated successfully using the discovered credentials.

The lab was marked as solved.

---

Impact

If exploited in a real-world application, attackers may:

- Bypass brute-force protections
- Discover user passwords
- Perform credential stuffing attacks
- Compromise user accounts
- Escalate privileges
- Achieve account takeover

---

Root Cause

The application relied on insufficient brute-force detection mechanisms.

Authentication controls focused on blocking obvious attack patterns but failed to prevent alternative attack strategies.

---

Mitigation

Recommended Security Measures

- Implement adaptive rate limiting
- Monitor authentication behavior rather than individual requests
- Enforce account lockout policies
- Use CAPTCHA after repeated failures
- Detect credential stuffing patterns
- Log and investigate suspicious login activity

---

OWASP Reference

- OWASP Top 10 (2021): A07 – Identification and Authentication Failures
- CWE-307: Improper Restriction of Excessive Authentication Attempts

---

Tools Used

- Burp Suite
- Burp Intruder
- Burp Repeater
- PortSwigger Web Security Academy

---

Key Learning

Brute-force protection is only effective when it addresses the underlying attack strategy rather than a single indicator such as request count or source IP.

Attackers frequently adapt their techniques to bypass weak protection mechanisms.

---

Disclaimer

This writeup is intended for educational purposes and authorized security testing only.