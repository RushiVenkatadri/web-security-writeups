# Referer-Based Access Control

## Overview

This lab demonstrates an access control vulnerability where the application relies on the HTTP `Referer` header to enforce authorization checks.

Since HTTP headers can be manipulated easily by attackers, trusting the `Referer` header for security decisions can lead to privilege escalation and unauthorized access.

This vulnerability is a form of Broken Access Control.

---

## Vulnerability Description

The application attempts to restrict access to administrative functionality by validating the `Referer` header.

Example:

```http
Referer: /admin
```

If the request contains the expected `Referer` value, the application processes the request.

Because attackers can modify HTTP headers using tools like Burp Suite, this protection can be bypassed easily.

---

## Lab Objective

Promote the user `carlos` to administrator.

---

## Steps Performed

### 1. Login to the Application

Logged into the lab using the provided credentials.

---

### 2. Observe Administrative Functionality

Using an administrator account, observed the request responsible for upgrading user roles.

Captured a request similar to:

```http
POST /admin-roles HTTP/1.1
```

with parameters:

```http
username=carlos&action=upgrade
```

---

### 3. Test Access Restrictions

Logged in using a lower-privileged account.

Attempted to replay the administrative request and observed that access was denied.

Analyzed the request and noticed the application relied on the `Referer` header for authorization validation.

---

### 4. Modify the Referer Header

Intercepted the request using Burp Suite Repeater.

Modified the `Referer` header to include an administrative path:

```http
Referer: https://lab-id.web-security-academy.net/admin
```

Modified request:

```http
POST /admin-roles HTTP/1.1
Referer: https://lab-id.web-security-academy.net/admin
```

---

### 5. Forward the Modified Request

Sent the modified request to the server.

The application trusted the manipulated `Referer` header and processed the request successfully.

---

### 6. Promote User to Administrator

The user `carlos` was successfully upgraded to administrator privileges.

The lab was marked as solved.

---

## Impact

If exploited in real-world applications, attackers may:

- Bypass access controls
- Escalate privileges
- Access administrative functionality
- Modify user roles
- Perform unauthorized actions
- Compromise sensitive application features

---

## Root Cause

The application trusted the client-controlled `Referer` header for authorization decisions.

HTTP headers can be modified easily and should never be relied upon as a primary security control.

---

## Mitigation

### Recommended Security Measures

- Enforce strict server-side authorization checks
- Never rely on client-controlled headers for security decisions
- Implement Role-Based Access Control (RBAC)
- Validate permissions independently of request headers
- Apply the principle of least privilege
- Monitor suspicious header manipulation attempts

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

Client-controlled headers such as `Referer` must never be trusted for authorization decisions.

Security controls should always be enforced server-side.

---

## Disclaimer

This writeup is intended for educational purposes and authorized security testing only.
