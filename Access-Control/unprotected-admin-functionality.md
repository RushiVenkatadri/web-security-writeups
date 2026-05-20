# Unprotected Admin Functionality

## Overview

Unprotected admin functionality is a critical access control vulnerability where administrative functions and sensitive operations are accessible without proper authentication or authorization checks. This allows unauthorized users to perform privileged actions they should never be able to execute.

## Vulnerability Description

When admin features are not properly protected, attackers can:
- Access sensitive administrative panels
- Modify system configurations
- Create, update, or delete user accounts
- View confidential information
- Disable security controls
- Escalate privileges

## Common Scenarios

### 1. **Direct URL Access**
Admin pages accessible directly without login:
```
GET /admin/dashboard
GET /admin/users
GET /admin/settings
```

### 2. **Hidden Admin Panels**
Admin interfaces accessible through predictable URLs:
```
GET /administrator.php
GET /admin-panel.html
GET /wp-admin/ (WordPress)
```

### 3. **Weak Authorization Checks**
Admin functions checking only if user is logged in, not if they're an admin:
```javascript
// Vulnerable: Only checks if logged in
if (user.isLoggedIn) {
    allowAdminAccess();
}

// Secure: Checks admin role
if (user.isLoggedIn && user.role === 'admin') {
    allowAdminAccess();
}
```

### 4. **Parameter Tampering**
Modifying user role parameters:
```
POST /profile/update
role=admin  (attacker modifies this)
```

## Real-World Examples

- **Case Study 1:** Admin panel accessible at `/admin` with no authentication
- **Case Study 2:** Admin API endpoints missing role verification
- **Case Study 3:** Admin functions checking only session existence, not permissions
    
## PortSwigger Lab Walkthrough

### Objective
Gain access to the admin panel and delete the user "carlos".

### Steps Performed
1. Logged into the lab application.
2. Checked robots.txt and discovered hidden admin endpoint.
3. Accessed `/administrator-panel`.
4. Navigated to admin interface.
5. Deleted the user "carlos".
6. Lab marked as solved.

### Impact
Attackers can gain unauthorized administrative access and perform sensitive actions without authentication.

### Mitigation
- Enforce server-side authorization checks
- Restrict admin endpoints
- Implement RBAC
- Monitor unauthorized admin access attempts



## OWASP Reference

- **OWASP Top 10 (2021):** A01:2021 – Broken Access Control
- **CWE-639:** Authorization Bypass Through User-Controlled Key

## Tools for Testing

- Burp Suite
- OWASP ZAP
- Postman
- curl

## Resources

- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [CWE-639: Authorization Bypass](https://cwe.mitre.org/data/definitions/639.html)

---

**Disclaimer:** Use this information only for authorized security testing and educational purposes.
