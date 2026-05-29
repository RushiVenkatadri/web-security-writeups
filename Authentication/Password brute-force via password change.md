Password Brute-Force via Password Change

Overview

This lab demonstrates how a vulnerable password change functionality can be abused to discover valid credentials and gain unauthorized access to user accounts.

Instead of attacking the login page directly, the attack targets the password change mechanism and leverages flaws in the application's validation logic.

This vulnerability is a form of Broken Authentication.

---

Vulnerability Description

Many applications require users to provide their current password before changing it.

A secure implementation should:

1. Verify the user's current password
2. Validate ownership of the account
3. Return generic error messages
4. Prevent brute-force attempts

If the application reveals whether the current password is correct through its responses, attackers may use the password change functionality as an alternative authentication oracle.

---

Lab Objective

Identify Carlos's password and gain access to his account.

---

Steps Performed

1. Login Using Valid Credentials

Authenticated using the provided account credentials.

Navigated to the account management section containing the password change functionality.

---

2. Analyze Password Change Requests

Intercepted the password change request using Burp Suite.

Observed parameters similar to:

POST /my-account/change-password HTTP/1.1

username=wiener
current-password=peter
new-password-1=test123
new-password-2=test123

---

3. Examine Validation Behavior

Modified request parameters and observed application responses.

Discovered that the application returned different responses depending on whether:

- The username was valid
- The current password was correct
- The new password validation failed

This behavior allowed the password change functionality to be used as a password validation mechanism.

---

4. Prepare Brute-Force Attack

Sent the request to Burp Intruder.

Configured a password wordlist against the current password parameter while targeting Carlos's account.

The application's responses were monitored for deviations indicating a successful password match.

---

5. Identify Valid Credentials

One request produced a unique response compared to all other attempts.

This indicated that the supplied password matched Carlos's current password.

---

6. Access Carlos's Account

Authenticated using Carlos's username and the discovered password.

The lab was marked as solved.

---

Impact

If exploited in a real-world application, attackers may:

- Discover valid user passwords
- Bypass login protections
- Compromise user accounts
- Circumvent rate limiting applied to login endpoints
- Gain unauthorized access to sensitive information
- Achieve complete account takeover

---

Root Cause

The application leaked authentication state information through password change responses.

By exposing whether the current password was correct, the application unintentionally created an alternative authentication endpoint that could be brute-forced.

---

Mitigation

Recommended Security Measures

- Return generic responses for password change failures
- Apply rate limiting to password change functionality
- Lock accounts after repeated failed attempts
- Require reauthentication for sensitive operations
- Monitor suspicious password change activity
- Ensure password validation does not reveal authentication state

---

OWASP Reference

- OWASP Top 10 (2021): A07 – Identification and Authentication Failures
- CWE-307: Improper Restriction of Excessive Authentication Attempts
- CWE-203: Observable Discrepancy

---

Tools Used

- Burp Suite
- Burp Intruder
- Burp Repeater
- PortSwigger Web Security Academy

---

Key Learning

Authentication weaknesses are not limited to login forms.

Any functionality that validates credentials—including password change workflows—can become a target for brute-force attacks if responses reveal too much information.

---

Disclaimer

This writeup is intended for educational purposes and authorized security testing only.