Broken Brute-Force Protection, Multiple Credentials per Request

Overview

This lab demonstrates how poorly designed brute-force protection mechanisms can be bypassed by submitting multiple credential attempts within a single HTTP request.

Although the application implements rate limiting and login protections, these defenses only monitor the number of requests rather than the number of authentication attempts contained within each request.

This vulnerability is a form of Broken Authentication.

---

Vulnerability Description

Many applications implement brute-force protection by limiting:

- Number of login requests
- Number of failed authentication attempts
- Login frequency from a single IP

However, if multiple credential combinations can be submitted within a single request, attackers may bypass these protections and test numerous passwords without triggering security controls.

As a result, the application becomes vulnerable to credential guessing attacks.

---

Lab Objective

Compromise Carlos's account by bypassing the application's brute-force protections.

---

Steps Performed

1. Analyze Login Functionality

Navigated to the login page and intercepted authentication requests using Burp Suite.

Observed a standard login request:

POST /login HTTP/1.1

username=carlos
password=test123

---

2. Observe Brute-Force Protection

Performed multiple failed login attempts.

The application enforced protection mechanisms after several unsuccessful requests.

These protections appeared to operate at the request level.

---

3. Investigate Request Parsing Behavior

Further analysis revealed that the application accepted multiple credential values within a single request.

Example:

username=carlos
password=test1
password=test2
password=test3
password=test4

The backend processed multiple authentication attempts while counting only one request against rate limiting controls.

---

4. Prepare Multi-Credential Request

Using Burp Suite Intruder, generated requests containing multiple password candidates.

Instead of sending hundreds of separate login requests, numerous password guesses were packed into a smaller number of requests.

---

5. Bypass Rate Limiting

Because the application's brute-force protections only tracked requests rather than individual password attempts, the attack successfully bypassed login restrictions.

The correct password was eventually identified.

---

6. Access Carlos's Account

Authenticated using the discovered credentials.

The lab was marked as solved.

---

Impact

If exploited in a real-world application, attackers may:

- Bypass brute-force protections
- Perform credential guessing attacks
- Discover user passwords
- Compromise user accounts
- Circumvent rate limiting mechanisms
- Achieve account takeover

---

Root Cause

The application incorrectly measured brute-force activity based on the number of HTTP requests instead of the number of authentication attempts.

As a result, attackers could submit multiple password guesses within a single request and evade detection.

---

Mitigation

Recommended Security Measures

- Count individual authentication attempts rather than requests
- Enforce account lockout policies
- Implement CAPTCHA after repeated failures
- Apply adaptive rate limiting
- Monitor anomalous authentication behavior
- Restrict duplicate authentication parameters
- Log excessive password validation attempts

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

Brute-force protection is only effective when it accurately measures authentication attempts.

Limiting requests alone is insufficient if attackers can perform multiple credential validations within a single request.

---

Disclaimer

This writeup is intended for educational purposes and authorized security testing only.