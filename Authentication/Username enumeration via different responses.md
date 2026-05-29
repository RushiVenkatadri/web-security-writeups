Username Enumeration via Different Responses

Overview

This lab demonstrates how inconsistent authentication responses can reveal valid usernames.

The application returns different error messages depending on whether the supplied username exists or whether the password is incorrect. By comparing these responses, attackers can identify valid accounts before attempting password attacks.

This vulnerability is a form of Information Disclosure and Broken Authentication.

---

Vulnerability Description

Authentication systems should avoid revealing whether a username exists within the application.

A secure implementation should return generic responses such as:

Invalid username or password

for all authentication failures.

When applications return different messages for:

- Invalid usernames
- Incorrect passwords
- Locked accounts

attackers can enumerate valid accounts and significantly improve brute-force attack effectiveness.

---

Lab Objective

Identify a valid username and gain access to Carlos's account.

---

Steps Performed

1. Analyze Login Functionality

Navigated to the application's login page and intercepted authentication requests using Burp Suite.

Example request:

POST /login HTTP/1.1

username=test
password=test123

---

2. Compare Authentication Responses

Submitted login attempts using invalid usernames and observed server responses.

Example:

Invalid username

---

3. Test Candidate Usernames

Performed authentication attempts against multiple usernames while supplying an incorrect password.

One response differed from the others.

Example:

Incorrect password

This indicated that the username existed but the password was incorrect.

---

4. Identify Valid Username

The response discrepancy revealed a valid account within the application.

The identified username was selected for further testing.

---

5. Discover Valid Password

Using the valid username, performed password testing until valid credentials were identified.

---

6. Access Carlos's Account

Authenticated successfully using the discovered credentials.

The lab was marked as solved.

---

Impact

If exploited in a real-world application, attackers may:

- Enumerate valid usernames
- Improve brute-force attack efficiency
- Conduct credential stuffing attacks
- Target high-value accounts
- Increase the likelihood of account compromise

---

Root Cause

The application exposed authentication state information through inconsistent error messages.

Different responses allowed attackers to distinguish valid users from invalid users.

---

Mitigation

Recommended Security Measures

- Return identical authentication responses for all failures
- Use generic login error messages
- Normalize response lengths and status codes
- Implement rate limiting and CAPTCHA controls
- Monitor repeated authentication failures
- Log enumeration attempts

---

OWASP Reference

- OWASP Top 10 (2021): A07 – Identification and Authentication Failures
- CWE-204: Observable Response Discrepancy
- CWE-203: Observable Discrepancy

---

Tools Used

- Burp Suite
- Burp Intruder
- Burp Repeater
- PortSwigger Web Security Academy

---

Key Learning

Authentication systems should never reveal whether a username exists.

Even small differences in login responses can provide attackers with valuable information and significantly increase the success rate of credential attacks.

---

Disclaimer

This writeup is intended for educational purposes and authorized security testing only.