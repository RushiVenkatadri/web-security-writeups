Broken Brute-Force Protection, Cookie Bypass

Overview

This lab demonstrates how authentication defenses can be bypassed by manipulating client-controlled cookies.

Although the application attempts to detect and block brute-force attacks, weaknesses in the implementation allow attackers to evade these protections and continue password guessing attempts.

This vulnerability is a form of Broken Authentication.

---

Vulnerability Description

Applications commonly implement brute-force protection mechanisms to defend against repeated login attempts.

Typical protections include:

- Account lockout
- Rate limiting
- IP blocking
- Temporary authentication restrictions

However, if the application stores authentication state or brute-force tracking information within client-controlled cookies, attackers may manipulate or reset these values to bypass security controls.

---

Lab Objective

Bypass the application's brute-force protection mechanism and gain access to Carlos's account.

---

Steps Performed

1. Analyze Authentication Functionality

Navigated to the login page and observed the application's behavior after multiple failed login attempts.

Authentication requests followed a standard format:

POST /login HTTP/1.1

username=carlos
password=test123

---

2. Trigger Brute-Force Protection

Performed repeated failed authentication attempts.

After several failures, the application activated defensive measures intended to block further password guessing attempts.

---

3. Inspect Session Cookies

Using Burp Suite, analyzed cookies generated during authentication.

Observed that certain client-side cookies appeared to track login state and brute-force protection behavior.

Example:

Cookie: session=<value>

---

4. Identify Weakness

Further testing revealed that modifying or replacing cookie values affected the application's brute-force tracking logic.

Because the application relied on client-controlled information, protections could be bypassed.

---

5. Reset Authentication State

Manipulated the relevant cookie values between authentication attempts.

This prevented the application from accurately tracking failed login attempts.

---

6. Perform Password Attack

Continued testing credentials while bypassing the application's lockout mechanism.

Eventually identified valid credentials for the target account.

---

7. Access Carlos's Account

Authenticated successfully using the discovered credentials.

The lab was marked as solved.

---

Impact

If exploited in a real-world application, attackers may:

- Bypass brute-force protections
- Perform unlimited password guessing attacks
- Discover valid credentials
- Compromise user accounts
- Circumvent security monitoring
- Achieve account takeover

---

Root Cause

The application trusted client-controlled cookie values when enforcing brute-force protection.

Because attackers could manipulate these values, authentication defenses became ineffective.

---

Mitigation

Recommended Security Measures

- Store authentication tracking data server-side
- Do not trust client-controlled security state
- Use secure session management
- Implement adaptive rate limiting
- Apply account lockout protections
- Monitor suspicious authentication activity
- Validate all authentication-related state on the server

---

OWASP Reference

- OWASP Top 10 (2021): A07 – Identification and Authentication Failures
- CWE-307: Improper Restriction of Excessive Authentication Attempts
- CWE-602: Client-Side Enforcement of Server-Side Security

---

Tools Used

- Burp Suite
- Burp Intruder
- Burp Repeater
- Browser Developer Tools
- PortSwigger Web Security Academy

---

Key Learning

Security controls must never rely on client-controlled data.

Authentication protections such as rate limiting and lockout mechanisms should always be enforced and tracked server-side to prevent manipulation by attackers.

---

Disclaimer

This writeup is intended for educational purposes and authorized security testing only.