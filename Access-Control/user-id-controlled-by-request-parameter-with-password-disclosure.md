# User ID Controlled by Request Parameter with Password Disclosure

## Overview

This lab demonstrates an access control vulnerability where sensitive user information is exposed through a user-controlled request parameter. By modifying the parameter value, an attacker can access another user's account details, including passwords.

This vulnerability is a classic example of Broken Access Control and insecure direct object reference (IDOR).

---

## Vulnerability Description

The application uses a request parameter to identify which user's account data should be displayed:

```http
GET /my-account?id=wiener
```

Since the server does not properly verify whether the authenticated user is authorized to access the requested account, attackers can modify the parameter and retrieve sensitive information belonging to other users.

In this lab, the administrator password is disclosed in the response.

---

## Lab Objective

Obtain the password for the user `administrator` and log into the administrator account.

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

The application used the `id` parameter to determine which account information to display.

---

### 3. Intercept the Request Using Burp Suite

Captured the request in Burp Suite Repeater.

Modified:

```http
id=wiener
```

to:

```http
id=administrator
```

Modified request:

```http
GET /my-account?id=administrator HTTP/1.1
```

---

### 4. Access Administrator Account Information

Forwarded the modified request to the server.

The application returned the administrator account details without performing proper authorization checks.

The response exposed sensitive information including the administrator password.

---

### 5. Login as Administrator

Used the disclosed administrator credentials to log into the administrator account.

Successfully solved the lab.

---

## Impact

If exploited in real-world applications, attackers may:

- Access sensitive user information
- Retrieve user passwords
- Perform account takeover
- Escalate privileges
- Compromise administrative accounts
- Access confidential data

---

## Root Cause

The application trusted user-controlled request parameters without validating whether the authenticated user was authorized to access the requested account.

Authorization checks were missing on the server side.

---

## Mitigation

### Recommended Security Measures

- Implement strict server-side authorization checks
- Validate resource ownership before returning data
- Never expose passwords in application responses
- Use indirect object references
- Implement Role-Based Access Control (RBAC)
- Apply the principle of least privilege

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

Applications must never trust user-controlled parameters for authorization decisions.

Even simple parameter manipulation can lead to sensitive data exposure and account compromise if proper access control checks are missing.

---

## Disclaimer

This writeup is intended for educational purposes and authorized security testing only.