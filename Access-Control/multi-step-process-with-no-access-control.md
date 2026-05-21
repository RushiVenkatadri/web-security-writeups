# Multi-Step Process with No Access Control on One Step

## Overview

This lab demonstrates an access control vulnerability where a multi-step administrative process fails to enforce authorization checks on one of the intermediate steps.

Although some parts of the workflow are protected, a missing authorization check on a specific step allows attackers to perform privileged actions.

This vulnerability is a form of Broken Access Control.

---

## Vulnerability Description

Applications often implement sensitive actions using multiple steps.

Example:

1. Access admin panel
2. Confirm action
3. Execute privileged operation

If authorization is not validated consistently during every step, attackers may directly access vulnerable endpoints and bypass restrictions.

---

## Lab Objective

Promote the user `carlos` to administrator.

---

## Steps Performed

### 1. Login to the Application

Logged into the lab using the provided credentials.

---

### 2. Observe the Admin Workflow

Using an administrator account, observed the request responsible for upgrading user privileges.

Captured a request similar to:

```http
POST /admin-roles HTTP/1.1
```

with parameters:

```http
username=carlos&action=upgrade
```

---

### 3. Switch to Lower-Privileged User

Logged in using a non-administrative account.

Attempted to access the admin panel directly but access was denied.

---

### 4. Intercept the Administrative Request

Used Burp Suite Repeater to replay the previously captured administrative request.

Modified request:

```http
POST /admin-roles HTTP/1.1
```

```http
username=carlos&action=upgrade
```

---

### 5. Forward the Request

Sent the modified request to the server.

The application processed the request successfully because authorization checks were missing for this step of the workflow.

---

### 6. Promote User to Administrator

The user `carlos` was successfully upgraded to administrator privileges.

The lab was marked as solved.

---

## Impact

If exploited in real-world applications, attackers may:

- Escalate privileges
- Access administrative functionality
- Modify user roles
- Bypass workflow restrictions
- Compromise sensitive application functions
- Fully compromise the application

---

## Root Cause

The application failed to enforce authorization checks consistently throughout the entire multi-step process.

Only the initial step was protected, while backend functionality remained exposed.

---

## Mitigation

### Recommended Security Measures

- Enforce authorization checks on every request
- Never trust workflow sequencing as a security control
- Validate permissions server-side for all sensitive actions
- Implement Role-Based Access Control (RBAC)
- Deny unauthorized requests by default
- Log and monitor privilege escalation attempts

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

Authorization must be enforced at every step of a sensitive workflow.

Protecting only the frontend or initial pages is not sufficient if backend endpoints remain accessible.

---

## Disclaimer

This writeup is intended for educational purposes and authorized security testing only.