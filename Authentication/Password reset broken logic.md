Password Reset Broken Logic

Overview

This lab demonstrates a flaw in the password reset workflow where the application fails to properly validate user identity during the password reset process.

Although the application provides a legitimate password recovery mechanism, weaknesses in the backend logic allow attackers to manipulate the process and reset another user's password.

This vulnerability is a form of Broken Authentication.

---

Vulnerability Description

Password reset functionality is often considered a trusted recovery mechanism for users who forget their credentials.

A secure password reset workflow should:

1. Verify the identity of the user requesting the reset
2. Generate a secure reset token
3. Bind the token to the correct account
4. Validate ownership before allowing password changes

If any of these checks are missing or improperly implemented, attackers may be able to reset passwords for accounts they do not own.

---

Lab Objective

Reset Carlos's password and gain access to his account.

---

Steps Performed

1. Analyze Password Reset Functionality

Navigated to the "Forgot Password" feature and observed the password recovery workflow.

The application generated password reset requests that could be inspected using Burp Suite.

---

2. Capture Reset Requests

Intercepted password reset traffic and examined how the application processed password recovery operations.

Observed parameters associated with user identification and password reset validation.

---

3. Identify Logic Weakness

Further analysis revealed that the application relied on user-controlled information during the password reset process.

The backend failed to correctly validate ownership of the reset operation.

---

4. Manipulate Reset Workflow

Modified the password reset request to target Carlos's account instead of the attacker's account.

Example request:

POST /forgot-password HTTP/1.1

username=carlos

The application processed the request without enforcing proper validation.

---

5. Complete Password Reset

Using the vulnerable workflow, successfully initiated and completed a password reset operation for Carlos's account.

A new password was assigned to the target account.

---

6. Authenticate as Carlos

Logged into the application using the newly configured credentials.

The lab was marked as solved.

---

Impact

If exploited in a real-world application, attackers may:

- Reset passwords for arbitrary users
- Gain unauthorized account access
- Compromise privileged accounts
- Bypass authentication controls
- Access sensitive information
- Achieve complete account takeover

---

Root Cause

The application failed to properly validate account ownership throughout the password reset workflow.

Critical security decisions relied on user-controlled parameters instead of trusted server-side validation.

---

Mitigation

Recommended Security Measures

- Use cryptographically secure reset tokens
- Bind tokens to a specific account and session
- Validate ownership before allowing password changes
- Expire reset tokens after use
- Implement rate limiting on password reset requests
- Log and monitor password recovery activity
- Require additional verification for sensitive accounts

---

OWASP Reference

- OWASP Top 10 (2021): A07 – Identification and Authentication Failures
- CWE-640: Weak Password Recovery Mechanism for Forgotten Password

---

Tools Used

- Burp Suite
- Burp Repeater
- Browser Developer Tools
- PortSwigger Web Security Academy

---

Key Learning

Password reset functionality is effectively an alternative authentication mechanism.

Any weakness in the password recovery process can be just as dangerous as a weakness in the login process and may directly lead to account takeover.

---

Disclaimer

This writeup is intended for educational purposes and authorized security testing only.