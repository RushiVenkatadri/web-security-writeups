# Business Logic Flaws

Business Logic Flaws are vulnerabilities that arise from errors in the design or implementation of an application's business logic. These flaws allow attackers to abuse the intended functionality of an application to achieve unauthorized results.

## Overview

Business Logic Flaws are often overlooked during security testing because they don't involve technical exploits but rather exploitation of the application's intended functionality. This section covers various business logic vulnerabilities and how to identify and prevent them.

## Topics Covered

- Price Manipulation
- Race Conditions
- Workflow Abuse
- Authorization Bypass through Logic Flaws
- Improper State Management
- Bonus and Reward Exploitation
- Cart and Checkout Abuse
- Account Enumeration
- Lack of Rate Limiting
- Insufficient Input Validation in Business Logic

## Common Vulnerabilities

- **Price Manipulation** - Modifying prices during checkout or using coupons incorrectly
- **Race Conditions** - Exploiting timing issues in concurrent transactions
- **Coupon/Discount Abuse** - Using coupons multiple times or in unintended ways
- **Cart Manipulation** - Adding items, modifying quantities, or changing prices in the cart
- **Inventory Exploitation** - Ordering items with zero inventory or negative quantities
- **Payment Bypass** - Completing purchases without proper payment
- **Workflow Abuse** - Skipping steps or performing actions out of order
- **Resource Exhaustion** - Generating unlimited resources or credits

## Real-World Examples

- **E-commerce:** Reducing product price by manipulating parameters before checkout
- **Banking:** Transferring money twice by exploiting race conditions
- **Social Media:** Increasing likes or followers through logic flaws
- **Airline:** Booking flights at unintended prices due to calculation errors
- **Loyalty Programs:** Exploiting bonus calculation logic to earn excessive points

## Detection Methods

1. **Manual Testing:**
   - Follow the intended user workflow
   - Try to perform actions out of order
   - Modify parameters at different stages
   - Test concurrent operations

2. **Automated Analysis:**
   - Monitor for unusual transactions
   - Track state changes in the application
   - Analyze business logic flow

3. **Code Review:**
   - Review calculation logic
   - Check authorization before business logic execution
   - Validate state transitions

## Remediation

### Best Practices

1. **Validate Business Logic**
   ```javascript
   // Vulnerable: Price taken from user input
   let price = req.body.price;
   
   // Secure: Price fetched from database
   let price = getProductPriceFromDB(productId);
   ```

2. **Implement Proper State Management**
   ```javascript
   // Check state before allowing action
   if (order.status !== 'pending') {
       return res.status(400).send('Invalid order state');
   }
   ```

3. **Use Rate Limiting**
   - Limit number of transactions per user
   - Prevent abuse of business logic

4. **Implement Concurrency Controls**
   - Use transactions with proper isolation levels
   - Implement locking mechanisms for critical operations

5. **Audit and Logging**
   - Log all business logic transactions
   - Monitor for unusual patterns
   - Alert on suspicious activities

6. **Test Business Logic Thoroughly**
   - Unit tests for business logic
   - Integration tests for workflows
   - Test edge cases and boundary conditions

## OWASP Reference

- **OWASP Top 10 (2021):** A04:2021 – Insecure Design
- **CWE-20:** Improper Input Validation
- **CWE-863:** Incorrect Authorization

## Tools for Testing

- Burp Suite
- OWASP ZAP
- Custom scripts for testing workflows
- Load testing tools for race conditions

## Resources

- [OWASP Business Logic Flaws](https://owasp.org/www-community/attacks/Business_logic_vulnerability)
- [CWE-863: Incorrect Authorization](https://cwe.mitre.org/data/definitions/863.html)

---

**Disclaimer:** Use this information only for authorized security testing and educational purposes.
