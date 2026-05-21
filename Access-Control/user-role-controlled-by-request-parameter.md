# User Role Controlled by Request Parameter

## Overview

This lab demonstrates a privilege escalation vulnerability where the application trusts user-controlled request parameters to determine administrative privileges.

By modifying a request parameter, an attacker can escalate privileges and gain unauthorized access to administrative functionality.

This is a classic example of Broken Access Control.

---

## Vulnerability Description

Applications should never trust client-controlled data for authorization decisions.

In this vulnerability, the application uses a request parameter such as:

```http
admin=false
```

An attacker can modify this parameter to:

```http
admin=true
```

This grants unauthorized administrative access because the server fails to properly validate user permissions.

---

## Lab Objective

Access the admin panel and delete the user `carlos`.

---

## Steps Performed

### 1. Login to the Application

Logged into the lab using the provided credentials.

---

### 2. Intercept Requests Using Burp Suite

Captured requests while navigating through the application.

Observed a request similar to:

```http
GET /my-account?id=wiener&admin=false HTTP/1.1
```

---

### 3. Modify the Request Parameter

Changed the parameter value from:

```http
admin=false
```

to:

```http
admin=true
```

Modified request:

```http
GET /my-account?id=wiener&admin=true HTTP/1.1
```

---

### 4. Forward the Request

Sent the modified request to the server.

The application granted administrative functionality.

---

### 5. Access the Admin Panel

Successfully accessed the admin interface.

Navigated to the user management section.

---

### 6. Delete User Carlos

Deleted the user:

```http
carlos
```

The lab was marked as solved.

---

## Impact

If exploited in real-world applications, attackers may:

- Gain unauthorized administrative access
- Modify application settings
- Delete or create users
- Access sensitive information
- Fully compromise the application

---

## Root Cause

The application trusted user-controlled request parameters for authorization decisions.

Authorization logic must always be enforced securely on the server side.

---

## Mitigation

### Recommended Security Measures

- Implement strict server-side authorization checks
- Never trust client-controlled parameters
- Use secure Role-Based Access Control (RBAC)
- Validate permissions on every sensitive request
- Log privilege escalation attempts
- Apply the principle of least privilege

---

## OWASP Reference

- OWASP Top 10 (2021): A01 – Broken Access Control
- CWE-639: Authorization Bypass Through User-Controlled Key

---

## Tools Used

- Burp Suite
- Browser Developer Tools
- PortSwigger Web Security Academy

---

## Key Learning

Authorization decisions must never rely on client-side parameters.

Even a single modifiable parameter can lead to full administrative compromise if server-side validation is missing.

---

## Disclaimer

This writeup is intended for educational purposes and authorized security testing only.