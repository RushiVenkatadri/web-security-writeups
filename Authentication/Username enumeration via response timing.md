Username Enumeration via Response Timing

Overview

This lab demonstrates how differences in server response times can reveal valid usernames.

Even when an application returns identical error messages for all authentication failures, subtle timing differences may allow attackers to determine whether a username exists.

This vulnerability is a form of Information Disclosure and Broken Authentication.

---

Vulnerability Description

Secure authentication systems should respond consistently regardless of whether:

- The username exists
- The password is incorrect
- The account is disabled

However, some applications perform additional processing for valid usernames, such as:

- Password hash verification
- Account status checks
- MFA validation

As a result, requests involving valid usernames may take slightly longer to process.

Attackers can measure these timing differences and use them to enumerate valid accounts.

---

Lab Objective

Identify a valid username using timing discrepancies and gain access to Carlos's account.

---

Steps Performed

1. Analyze Login Functionality

Intercepted login requests using Burp Suite.

Example request:

POST /login HTTP/1.1

username=test
password=test123

---

2. Establish Baseline Responses

Submitted multiple login attempts using invalid usernames.

Observed the average response time returned by the application.

These requests were processed quickly because the application immediately rejected non-existent accounts.

---

3. Test Candidate Usernames

Using Burp Intruder, submitted authentication requests against a list of usernames.

A long random password was used for each request.

Example:

username=carlos
password=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAA

---

4. Measure Response Times

Compared the server response times for each request.

One username consistently generated slightly longer responses than the others.

This suggested that the application performed additional processing for that account.

---

5. Identify Valid Username

The timing anomaly revealed the existence of a valid account.

The identified username was used for further authentication testing.

---

6. Discover Valid Credentials

Performed password testing against the identified account.

Successfully recovered valid credentials and authenticated.

---

7. Access Carlos's Account

Logged into Carlos's account.

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

The application performed different backend operations depending on whether the username existed.

These differences created measurable timing discrepancies that leaked account existence information.

---

Mitigation

Recommended Security Measures

- Normalize authentication processing time
- Perform identical operations for valid and invalid usernames
- Implement constant-time authentication logic where possible
- Apply rate limiting and CAPTCHA protections
- Monitor enumeration attempts
- Use generic authentication responses

---

OWASP Reference

- OWASP Top 10 (2021): A07 – Identification and Authentication Failures
- CWE-208: Observable Timing Discrepancy

---

Tools Used

- Burp Suite
- Burp Intruder
- Burp Repeater
- PortSwigger Web Security Academy

---

Key Learning

Authentication responses can leak information even when visible error messages appear identical.

Response timing analysis is a powerful technique for identifying valid accounts and should always be considered during authentication testing.

---

Disclaimer

This writeup is intended for educational purposes and authorized security testing only.