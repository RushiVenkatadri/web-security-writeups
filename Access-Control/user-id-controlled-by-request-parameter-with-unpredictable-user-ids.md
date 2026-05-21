# User ID Controlled by Request Parameter with Unpredictable User IDs

## Overview

This lab demonstrates an access control vulnerability where user account information can still be accessed even though the application uses unpredictable user identifiers instead of sequential usernames.

Although the application attempts to make user enumeration difficult by using random-looking identifiers, authorization checks are still missing.

This vulnerability is a form of Broken Access Control and insecure direct object reference (IDOR).

---

## Vulnerability Description

The application uses unpredictable user IDs such as GUIDs or UUIDs to identify user accounts:

```http
GET /my-account?id=3f8c7e92-8b2f-4d7f-a1d2
```

Developers sometimes assume that unpredictable identifiers alone provide security.

However, if proper authorization checks are missing, attackers can still access other users' data once valid identifiers are discovered.

---

## Lab Objective

Obtain the API key for the user `carlos`.

---

## Steps Performed

### 1. Login to the Application

Logged into the lab using the provided credentials.

---

### 2. Observe User Identifiers

Navigated through the application and observed that user accounts were referenced using unpredictable IDs instead of usernames.

Example:

```http
GET /my-account?id=8f2a91b7-d2e3-4a5b
```

---

### 3. Discover Another User's Identifier

Browsed the application and identified a blog post authored by `carlos`.

Opened the post and observed that the user ID associated with `carlos` was exposed within the page source or request.

---

### 4. Intercept the Request Using Burp Suite

Captured the account request in Burp Suite Repeater.

Modified the request parameter from the current user ID to Carlos's identifier.

Modified request:

```http
GET /my-account?id=carlos-user-id HTTP/1.1
```

---

### 5. Access Carlos's Account Information

Forwarded the modified request.

The application returned Carlos's account details without validating authorization.

Sensitive information including the API key was exposed.

---

### 6. Retrieve the API Key

Copied the exposed API key from the response.

Submitted the API key to complete the lab.

Successfully solved the lab.

---

## Impact

If exploited in real-world applications, attackers may:

- Access sensitive user information
- Retrieve API keys or tokens
- Perform account takeover
- Escalate privileges
- Access confidential data
- Compromise user privacy

---

## Root Cause

The application relied on unpredictable identifiers as a security mechanism instead of enforcing proper authorization checks.

Security through obscurity alone is not sufficient.

---

## Mitigation

### Recommended Security Measures

- Enforce strict server-side authorization checks
- Validate ownership of requested resources
- Never rely solely on unpredictable identifiers
- Implement Role-Based Access Control (RBAC)
- Apply the principle of least privilege
- Monitor unauthorized access attempts

---

## OWASP Reference

- OWASP Top 10 (2021): A01 – Broken Access Control
- CWE-639: Authorization Bypass Through User-Controlled Key

---

## Tools Used

- Burp Suite
- Burp Repeater
- Browser Developer Tools
- PortSwigger Web Security Academy

---

## Key Learning

Unpredictable identifiers do not replace proper access control.

Applications must always validate whether the authenticated user is authorized to access requested resources.

---

## Disclaimer

This writeup is intended for educational purposes and authorized security testing only.