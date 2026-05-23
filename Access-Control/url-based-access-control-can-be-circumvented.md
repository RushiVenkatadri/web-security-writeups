# URL-Based Access Control Can Be Circumvented

## Overview

This lab demonstrates an access control vulnerability where restrictions based on URL paths can be bypassed by manipulating HTTP headers.

The application attempts to block access to administrative functionality, but the backend server still processes requests when specific headers are added.

This vulnerability is a form of Broken Access Control.

---

## Vulnerability Description

Applications sometimes implement access restrictions at the frontend level while backend endpoints remain accessible internally.

In this lab, direct access to the admin panel is blocked:

```http
GET /admin
```

However, by using a special HTTP header, the request can be rewritten internally and bypass the access restrictions.

---

## Lab Objective

Access the admin panel and delete the user `carlos`.

---

## Steps Performed

### 1. Login to the Application

Logged into the lab using the provided credentials.

---

### 2. Attempt Direct Access to the Admin Panel

Tried accessing the following endpoint:

```http
GET /admin
```

The application returned an unauthorized response.

---

### 3. Intercept the Request Using Burp Suite

Captured the request using Burp Suite Repeater.

Observed that direct access to the administrative endpoint was restricted.

---

### 4. Add the Rewrite Header

Modified the request by adding the following header:

```http
X-Original-URL: /admin
```

Modified request:

```http
GET / HTTP/1.1
X-Original-URL: /admin
```

---

### 5. Access the Admin Panel

Forwarded the modified request.

The backend server trusted the `X-Original-URL` header and internally rewrote the request to the restricted admin endpoint.

Successfully gained access to the admin panel.

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

- Bypass access restrictions
- Access hidden administrative functionality
- Escalate privileges
- Delete or modify users
- Access sensitive information
- Fully compromise the application

---

## Root Cause

The application relied on frontend or proxy-level URL restrictions while the backend trusted rewrite headers such as:

```http
X-Original-URL
```

Improper trust in internal routing headers allowed attackers to bypass access controls.

---

## Mitigation

### Recommended Security Measures

- Enforce authorization checks on the backend server
- Never trust client-controlled rewrite headers
- Restrict internal routing headers at proxies
- Implement Role-Based Access Control (RBAC)
- Validate permissions before processing requests
- Log suspicious header manipulation attempts

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

Frontend restrictions alone are not sufficient for security.

Authorization must always be enforced securely on the backend server regardless of proxy or routing behavior.

---

## Disclaimer

This writeup is intended for educational purposes and authorized security testing only.
