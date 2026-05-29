Username Enumeration via Subtly Different Responses

Overview

This lab demonstrates how small differences in authentication responses can reveal valid usernames.

Unlike obvious username enumeration vulnerabilities, the application attempts to return similar error messages. However, subtle variations in response length, formatting, or content allow attackers to distinguish valid users from invalid ones.

This vulnerability is a form of Information Disclosure and Broken Authentication.

---

Vulnerability Description

Applications often attempt to prevent username enumeration by returning generic login errors.

Example:

Invalid username or password

However, even when messages appear identical, subtle differences may still exist in:

- Response length
- Whitespace
- HTML content
- Redirect behavior
- Response headers

Attackers can analyze these discrepancies to identify valid usernames.

---

Lab Objective

Identify a valid username and gain access to Carlos's account.

---

Steps Performed

1. Analyze Authentication Requests

Intercepted login requests using Burp Suite.

Example request:

POST /login HTTP/1.1

username=test
password=test123

---

2. Prepare Username Enumeration Attack

Sent the login request to Burp Intruder.

Configured a payload list containing candidate usernames.

Used a known invalid password for all attempts.

---

3. Compare Application Responses

Executed the attack and compared responses based on:

- Status codes
- Response lengths
- Returned content

Although the application displayed similar error messages, one username generated a slightly different response.

---

4. Identify Valid Username

The anomalous response indicated that the username existed within the application.

This allowed identification of the target account.

---

5. Discover Valid Password

Performed further testing using the identified username.

A password attack eventually revealed valid credentials.

---

6. Access Carlos's Account

Authenticated using the discovered credentials.

The lab was marked as solved.

---

Impact

If exploited in a real-world application, attackers may:

- Enumerate valid usernames
- Improve brute-force attack efficiency
- Facilitate credential stuffing attacks
- Target specific users
- Increase the likelihood of account compromise

---

Root Cause

Although the application attempted to use generic authentication responses, subtle implementation differences leaked information about account existence.

---

Mitigation

Recommended Security Measures

- Return identical responses for all authentication failures
- Normalize response lengths
- Use consistent status codes
- Avoid conditional error messages
- Monitor enumeration attempts
- Implement rate limiting and CAPTCHA controls

---

OWASP Reference

- OWASP Top 10 (2021): A07 – Identification and Authentication Failures
- CWE-204: Observable Response Discrepancy
- CWE-203: Observable Discrepancy

---

Tools Used

- Burp Suite
- Burp Intruder
- Burp Comparer
- PortSwigger Web Security Academy

---

Key Learning

Even tiny response differences can reveal sensitive information.

When testing authentication systems, comparing response lengths and subtle variations is often more effective than focusing only on visible error messages.

---

Disclaimer

This writeup is intended for educational purposes and authorized security testing only.