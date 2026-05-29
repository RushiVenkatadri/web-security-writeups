2FA Broken Logic

Overview

This lab demonstrates a flaw in the implementation of Two-Factor Authentication (2FA).

Although the application requires users to complete a second authentication step, improper validation of the authentication workflow allows attackers to bypass the intended security controls and gain unauthorized access.

This vulnerability is a form of Broken Authentication.

---

Vulnerability Description

Two-Factor Authentication is intended to provide an additional layer of security after a user successfully enters valid credentials.

A secure authentication workflow should verify:

1. Username and password
2. Ownership of the second authentication factor

If the application fails to correctly bind the second factor to the authenticated user session, attackers may bypass verification and gain access to protected accounts.

---

Lab Objective

Access Carlos's account by exploiting flaws in the two-factor authentication process.

---

Steps Performed

1. Login Using Known Credentials

Logged into the application using the provided credentials.

During authentication, the application redirected the user to a second-factor verification page.

---

2. Analyze Authentication Flow

Using Burp Suite, intercepted requests involved in the login process.

Observed that the application relied on a parameter identifying the user currently undergoing two-factor verification.

---

3. Modify User Context

Intercepted the request responsible for submitting the verification code.

Modified the user identifier to reference Carlos instead of the original authenticated user.

Example request:

POST /login2 HTTP/1.1

mfa-code=1234

---

4. Complete Verification Process

Forwarded the modified request.

The application failed to validate that the second-factor verification process belonged to the authenticated session.

---

5. Access Carlos's Account

The authentication process completed successfully for Carlos's account.

The lab was marked as solved.

---

Impact

If exploited in a real-world application, attackers may:

- Bypass Two-Factor Authentication
- Gain unauthorized account access
- Compromise sensitive user information
- Escalate privileges
- Circumvent additional security controls
- Achieve complete account takeover

---

Root Cause

The application failed to properly bind the second-factor authentication process to the authenticated user session.

As a result, user identity information could be manipulated during the verification stage.

---

Mitigation

Recommended Security Measures

- Bind MFA verification to the authenticated session
- Never trust client-supplied user identifiers
- Validate ownership of the MFA challenge server-side
- Generate unpredictable MFA session tokens
- Expire authentication states after failed verification attempts
- Log suspicious MFA manipulation attempts

---

OWASP Reference

- OWASP Top 10 (2021): A07 – Identification and Authentication Failures
- CWE-287: Improper Authentication

---

Tools Used

- Burp Suite
- Burp Repeater
- Browser Developer Tools
- PortSwigger Web Security Academy

---

Key Learning

Two-Factor Authentication only improves security when the verification process is securely tied to the authenticated session.

Any opportunity to manipulate user context during MFA validation can completely undermine the protection offered by 2FA.

---

Disclaimer

This writeup is intended for educational purposes and authorized security testing only.