# Unprotected Admin Functionality with Unpredictable URL

## Overview

This lab demonstrates an access control vulnerability where sensitive administrative functionality is hidden behind an unpredictable URL instead of being properly protected with authentication and authorization checks.

Although the admin interface is not directly linked within the application, attackers can still discover and access it.

This vulnerability is a form of Broken Access Control.

---

## Vulnerability Description

Some applications attempt to secure administrative functionality by hiding it behind obscure or unpredictable URLs.

Example:

```http
/admin-x7f92
```

Developers may assume attackers cannot access the functionality if the URL is difficult to guess.

However, hidden URLs are not a substitute for proper authorization checks.

If the endpoint is discovered, attackers may gain full administrative access.

---

## Lab Objective

Access the hidden admin panel and delete the user `carlos`.

---

## Steps Performed

### 1. Login to the Application

Logged into the lab using the provided credentials.

---

### 2. Analyze the Application Source

Inspected the application source code and JavaScript files.

Discovered a hidden administrative endpoint within the source:

```http
/admin-3a7f1c
```

The URL was not visible through normal application navigation.

---

### 3. Access the Hidden Admin Panel

Navigated directly to the discovered endpoint:

```http
GET /admin-3a7f1c HTTP/1.1
```

The application granted access to the administrator interface without requiring administrative privileges.

---

### 4. Locate User Management Functionality

Inside the admin panel, identified functionality responsible for managing users.

Observed the delete action for the user:

```http
carlos
```

---

### 5. Delete the User Carlos

Triggered the delete functionality for the target user.

The server processed the request successfully.

The user `carlos` was deleted.

The lab was marked as solved.

---

## Impact

If exploited in real-world applications, attackers may:

- Access hidden administrative interfaces
- Delete or modify users
- Access sensitive information
- Escalate privileges
- Modify application settings
- Fully compromise the application

---

## Root Cause

The application relied on URL obscurity instead of implementing proper server-side authorization checks.

Security through obscurity alone is not an effective security control.

---

## Mitigation

### Recommended Security Measures

- Enforce strict server-side authorization checks
- Never rely on hidden or unpredictable URLs for security
- Implement Role-Based Access Control (RBAC)
- Restrict administrative endpoints properly
- Validate permissions on every sensitive request
- Log and monitor unauthorized access attempts

---

## OWASP Reference

- OWASP Top 10 (2021): A01 – Broken Access Control
- CWE-285: Improper Authorization

---

## Tools Used

- Burp Suite
- Browser Developer Tools
- PortSwigger Web Security Academy

---

## Key Learning

Hidden URLs do not provide real security.

Administrative functionality must always be protected using proper authentication and authorization mechanisms.

---

## Disclaimer

This writeup is intended for educational purposes and authorized security testing only.