# User ID Controlled by Request Parameter with Data Leakage in Redirect

## Overview

This lab demonstrates an access control vulnerability where sensitive user information is exposed through a redirect response after manipulating a user-controlled request parameter.

Although the application attempts to redirect unauthorized users, sensitive data is still leaked within the redirect response.

This vulnerability is a form of Broken Access Control and insecure direct object reference (IDOR).

---

## Vulnerability Description

The application uses a request parameter to identify user accounts:

```http
GET /my-account?id=wiener
```

An attacker can modify the parameter value to access another user's account.

Although the application redirects unauthorized users, sensitive information is still included in the response body, resulting in information disclosure.

---

## Lab Objective

Obtain the API key for the user `carlos`.

---

## Steps Performed

### 1. Login to the Application

Logged into the lab using the provided credentials.

---

### 2. Navigate to "My Account"

Observed the following request:

```http
GET /my-account?id=wiener HTTP/1.1
```

The application used the `id` parameter to determine which account information should be displayed.

---

### 3. Intercept the Request Using Burp Suite

Captured the request using Burp Suite Repeater.

Modified:

```http
id=wiener
```

to:

```http
id=carlos
```

Modified request:

```http
GET /my-account?id=carlos HTTP/1.1
```

---

### 4. Observe the Redirect Response

Forwarded the modified request.

The application responded with a redirect indicating unauthorized access.

However, sensitive information belonging to the user `carlos` was still included within the response body.

The response leaked the API key for the target user.

---

### 5. Retrieve the API Key

Copied the exposed API key from the response.

Submitted the API key to complete the lab.

Successfully solved the lab.

---

## Impact

If exploited in real-world applications, attackers may:

- Access sensitive user information
- Retrieve API keys or authentication tokens
- Perform account takeover
- Escalate privileges
- Access confidential data
- Compromise user accounts

---

## Root Cause

The application failed to properly protect sensitive information during redirect responses.

Although access restrictions existed, sensitive data was still processed and leaked before authorization enforcement was completed.

---

## Mitigation

### Recommended Security Measures

- Enforce strict server-side authorization checks
- Never include sensitive data in unauthorized responses
- Validate user permissions before processing requests
- Use indirect object references
- Implement Role-Based Access Control (RBAC)
- Apply secure response handling practices

---

## OWASP Reference

- OWASP Top 10 (2021): A01 – Broken Access Control
- CWE-200: Exposure of Sensitive Information to an Unauthorized Actor

---

## Tools Used

- Burp Suite
- Burp Repeater
- Browser Developer Tools
- PortSwigger Web Security Academy

---

## Key Learning

Applications must validate authorization before processing sensitive data.

Even redirect responses can unintentionally leak confidential information if access control checks are implemented incorrectly.

---

## Disclaimer

This writeup is intended for educational purposes and authorized security testing only.