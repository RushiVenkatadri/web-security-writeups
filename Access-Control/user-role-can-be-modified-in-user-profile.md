# User Role Can Be Modified in User Profile

## Overview

This lab demonstrates a privilege escalation vulnerability where a user can modify their own role by tampering with parameters in a profile update request.

The application trusts client-controlled input and fails to properly validate authorization on the server side.

This vulnerability is a form of Broken Access Control.

---

## Vulnerability Description

Applications should never allow users to control authorization-related parameters.

In this lab, the application includes a role-related parameter within the profile update request:

```http
roleid=1
```

By modifying this parameter, attackers can escalate their privileges and gain administrative access.

---

## Lab Objective

Gain access to the administrator panel and delete the user `carlos`.

---

## Steps Performed

### 1. Login to the Application

Logged into the lab using the provided credentials.

---

### 2. Navigate to "My Account"

Accessed the account settings page and updated profile information.

Observed a request similar to:

```http
POST /my-account/change-email HTTP/1.1
```

with parameters:

```http
email=test@example.com
roleid=1
```

---

### 3. Intercept the Request Using Burp Suite

Captured the request using Burp Suite Repeater.

Observed that the request contained a user role parameter:

```http
roleid=1
```

Modified the parameter value to:

```http
roleid=2
```

---

### 4. Forward the Modified Request

Sent the modified request to the server.

The application processed the request successfully without validating whether the user was authorized to modify role information.

---

### 5. Gain Administrative Access

After updating the role parameter, administrative functionality became accessible.

Navigated to the admin panel successfully.

---

### 6. Delete User Carlos

Located the user management functionality and deleted the user:

```http
carlos
```

The lab was marked as solved.

---

## Impact

If exploited in real-world applications, attackers may:

- Escalate privileges
- Gain unauthorized administrative access
- Modify user roles
- Access sensitive functionality
- Delete or modify users
- Fully compromise the application

---

## Root Cause

The application trusted user-controlled input for authorization decisions.

Role-related parameters should never be modifiable by unprivileged users.

---

## Mitigation

### Recommended Security Measures

- Enforce strict server-side authorization checks
- Never trust client-controlled role parameters
- Implement Role-Based Access Control (RBAC)
- Restrict modification of sensitive attributes
- Validate permissions before processing requests
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

Authorization decisions must always be enforced securely on the server side.

Allowing users to control role-related parameters can directly lead to privilege escalation and administrative compromise.

---

## Disclaimer

This writeup is intended for educational purposes and authorized security testing only.
