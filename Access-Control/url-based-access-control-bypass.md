# URL-Based Access Control Bypass

## Overview
URL-based access control bypass occurs when sensitive functionality is protected only by the visible URL structure instead of proper server-side authorization checks.

Attackers can directly access restricted endpoints by modifying URLs or navigating to hidden administrative paths.

---

## Vulnerability Description
In this lab, administrative functionality was restricted through the user interface but not properly enforced on the server side.

By modifying the requested URL, it was possible to bypass access restrictions and gain unauthorized access to privileged functionality.

---

## PortSwigger Lab Walkthrough

### Objective
Access the administrator panel and perform administrative actions without proper authorization.

### Steps Performed
1. Logged into the lab application using a normal user account.
2. Intercepted requests using Burp Suite.
3. Observed restricted functionality hidden from standard users.
4. Modified the requested URL to target the admin endpoint directly.
5. Successfully bypassed access control restrictions.
6. Accessed administrative functionality without admin privileges.
7. Completed the lab objective.

---

## Impact
This vulnerability can allow attackers to:
- Access sensitive administrative functionality
- Perform unauthorized actions
- Escalate privileges
- Modify or delete sensitive data

---

## Mitigation
- Implement proper server-side authorization checks
- Enforce role-based access control (RBAC)
- Do not rely on hidden URLs for security
- Validate permissions for every sensitive request

---

## Tools Used
- Burp Suite
- HTTP Request Manipulation

---

## Key Learning
This lab helped me understand how relying only on URL restrictions without proper authorization checks can lead to serious access control vulnerabilities.

---

## OWASP Reference
OWASP Top 10 2021 – Broken Access Control

---

## Disclaimer
This writeup is created for educational purposes and authorized security training only.