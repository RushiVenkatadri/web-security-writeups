# Offline Password Cracking

Overview

This lab demonstrates how weak password storage mechanisms can allow attackers to recover user credentials through offline password cracking.

The application stores password-related information in a format that can be obtained and analyzed outside the application, enabling attackers to perform password guessing attacks without interacting with the target server.

This vulnerability is a form of Broken Authentication.

---

Vulnerability Description

Passwords should never be stored in plaintext and should always be protected using strong one-way hashing algorithms.

If an attacker gains access to password hashes or equivalent authentication data, they can perform offline attacks against the data using password cracking tools and wordlists.

Unlike online brute-force attacks, offline cracking is not limited by rate limits, account lockouts, or intrusion detection mechanisms.

---

Lab Objective

Recover Carlos's password and access his account.

---

Steps Performed

1. Analyze Authentication Mechanism

Logged into the application using the provided credentials and explored account-related functionality.

During testing, authentication-related data was identified that could be used for offline analysis.

---

2. Obtain Password-Related Data

Using Burp Suite and application functionality, extracted authentication information associated with a user account.

The application exposed data that could be analyzed without further interaction with the target server.

---

3. Identify Password Format

Examined the retrieved data and determined that it represented a password hash.

This hash could be subjected to offline password cracking techniques.

---

4. Perform Offline Analysis

Used password cracking techniques and common password dictionaries to recover the original password from the obtained hash.

Since the attack occurred offline, no account lockouts or rate limiting controls were triggered.

---

5. Recover Valid Credentials

Successfully identified the user's password.

The recovered credentials were then used to authenticate to the target account.

---

6. Access Carlos's Account

Logged into Carlos's account using the recovered password.

The lab was marked as solved.

---

Impact

If exploited in a real-world application, attackers may:

- Recover user passwords
- Perform large-scale credential attacks
- Compromise multiple accounts
- Reuse recovered credentials on other platforms
- Escalate privileges
- Achieve complete account takeover

---

Root Cause

The application exposed password-related information that could be analyzed offline.

Combined with weak password choices or insufficient password protection mechanisms, this allowed credentials to be recovered.

---

Mitigation

Recommended Security Measures

- Use strong password hashing algorithms such as Argon2, bcrypt, or scrypt
- Apply unique salts to every password
- Enforce strong password policies
- Protect authentication data from unauthorized access
- Implement credential monitoring and breach detection
- Require Multi-Factor Authentication (MFA)

---

OWASP Reference

- OWASP Top 10 (2021): A07 – Identification and Authentication Failures
- CWE-916: Use of Password Hash With Insufficient Computational Effort

---

Tools Used

- Burp Suite
- Burp Decoder
- Password Cracking Utilities
- PortSwigger Web Security Academy

---

Key Learning

Password security does not end at authentication.

Even if login functionality is secure, exposed password hashes or authentication data can allow attackers to recover credentials through offline attacks where traditional protections such as rate limiting and account lockouts provide no defense.

---

Disclaimer

This writeup is intended for educational purposes and authorized security testing only.