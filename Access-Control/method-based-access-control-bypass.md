# Method-Based Access Control Bypass

## Overview

This lab demonstrates an access control vulnerability where the application enforces restrictions for one HTTP method but fails to apply the same restrictions to another method.

An attacker can bypass access controls simply by changing the HTTP request method.

This vulnerability is a form of Broken Access Control.

---

## Vulnerability Description

Applications sometimes restrict sensitive functionality only for specific HTTP methods.

Example:

```http
GET /admin/deleteUser
```

may be blocked, while:

```http
POST /admin/deleteUser
```

is still accessible.

If authorization checks are inconsistent across request methods, attackers can bypass restrictions by modifying the HTTP method.

---

## Lab Objective

Promote the user `carlos` to administrator.

---

## Steps Performed

### 1. Login to the Application

Logged into the lab using the provided credentials.

---

### 2. Observe Administrative Functionality

Browsed the application and identified an admin panel functionality responsible for upgrading user roles.

Observed the following request:

```http
POST /admin-roles HTTP/1.1
```

with parameters similar to:

```http
username=carlos&action=upgrade
```

---

### 3. Test Access Restrictions

Attempted to access the functionality using a lower-privileged account.

The application blocked the request.

---

### 4. Intercept the Request Using Burp Suite

Captured the request using Burp Suite Repeater.

Modified the HTTP method from:

```http
POST
```

to:

```http
GET
```

Modified request:

```http
GET /admin-roles?username=carlos&action=upgrade HTTP/1.1
```

---

### 5. Forward the Modified Request

Sent the modified request to the server.

The server processed the request successfully because access control validation was missing for the modified HTTP method.

---

### 6. Promote User to Administrator

The user `carlos` was successfully upgraded to administrator.

The lab was marked as solved.

---

## Impact

If exploited in real-world applications, attackers may:

- Bypass access restrictions
- Escalate privileges
- Access administrative functionality
- Modify sensitive data
- Compromise user accounts
- Fully compromise the application

---

## Root Cause

The application enforced authorization checks inconsistently across HTTP methods.

Access controls were validated for one method but missing for another.

---

## Mitigation

### Recommended Security Measures

- Apply authorization checks consistently across all HTTP methods
- Restrict sensitive functionality server-side
- Deny requests by default
- Implement Role-Based Access Control (RBAC)
- Validate permissions before processing requests
- Monitor suspicious method tampering attempts

---

## OWASP Reference

- OWASP Top 10 (2021): A01 – Broken Access Control
- CWE-285: Improper Authorization

---

## Tools Used

- Burp Suite
- Burp Repeater
- Browser Developer Tools
- PortSwigger Web Security Academy

---

## Key Learning

Authorization checks must be enforced consistently across all request methods.

Changing an HTTP method should never bypass security restrictions.

---

## Disclaimer

This writeup is intended for educational purposes and authorized security testing only.