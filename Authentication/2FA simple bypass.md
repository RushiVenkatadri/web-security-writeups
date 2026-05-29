2FA Simple Bypass

Overview

This lab demonstrates how an application can fail to properly enforce Two-Factor Authentication (2FA), allowing attackers to bypass the second authentication step and gain unauthorized access.

Although the application requires users to enter a verification code after logging in, flaws in the authentication workflow allow attackers to access another user's account without completing the intended verification process.

This vulnerability is a form of Broken Authentication.

---

Vulnerability Description

Two-Factor Authentication is designed to strengthen account security by requiring:

1. Something the user knows (password)
2. Something the user has (verification code)

A secure implementation must ensure that the second factor is correctly tied to the authenticated user's session.

If the application fails to validate this relationship, attackers may bypass 2FA and gain unauthorized access.

---

Lab Objective

Access Carlos's account by exploiting weaknesses in the two-factor authentication process.

---

Steps Performed

1. Login Using Known Credentials

Authenticated using the provided credentials.

After successful login, the application redirected the user to a second-factor verification page.

---

2. Analyze Authentication Workflow

Using Burp Suite, observed the authentication process and identified requests responsible for handling two-factor verification.

The application relied on user-related information during the verification stage.

---

3. Modify Authentication Context

Intercepted requests involved in the 2FA workflow and analyzed how user identity was maintained between authentication steps.

Discovered that the application did not properly enforce ownership checks during verification.

---

4. Bypass Two-Factor Verification

Manipulated the authentication process to target another user account.

The application failed to ensure that the verification process belonged to the authenticated session.

As a result, access was granted without completing the intended second authentication factor.

---

5. Access Carlos's Account

Successfully authenticated as Carlos.

The lab was marked as solved.

---

Impact

If exploited in a real-world application, attackers may:

- Bypass Two-Factor Authentication
- Gain unauthorized account access
- Compromise sensitive user information
- Circumvent security controls
- Escalate privileges
- Achieve complete account takeover

---

Root Cause

The application failed to correctly bind the two-factor authentication process to the authenticated user session.

Critical authentication decisions relied on insufficient validation of user context.

---

Mitigation

Recommended Security Measures

- Bind MFA challenges to authenticated sessions
- Validate ownership of all MFA requests server-side
- Use secure session identifiers
- Prevent manipulation of user identifiers during authentication
- Expire incomplete authentication workflows
- Monitor suspicious MFA activity

---

OWASP Reference

- OWASP Top 10 (2021): A07 – Identification and Authentication Failures
- CWE-287: Improper Authentication
- CWE-306: Missing Authentication for Critical Function

---

Tools Used

- Burp Suite
- Burp Repeater
- Browser Developer Tools
- PortSwigger Web Security Academy

---

Key Learning

Two-Factor Authentication only provides security when every stage of the authentication workflow is properly enforced.

Weak session validation can completely undermine the protection offered by MFA and lead directly to account compromise.

---

Disclaimer

This writeup is intended for educational purposes and authorized security testing only.