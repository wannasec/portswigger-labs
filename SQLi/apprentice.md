# SQL Injection Labs PortSwigger

### Apprentice
**Completed:** 2/2

---

### 1. SQL injection in WHERE clause allowing retrieval of hidden data

**Goal:** Retrieve products that should be hidden.

**Vulnerability:** SQL injection in the `category` parameter of a product filter.

**Idea:** Manipulate the `WHERE` clause so the condition is always true.

**Payload:** ' OR 1=1--

**Result:** The query returns all products, including unreleased ones.

---

### 2. SQL injection allowing login bypass

**Goal:** Log in as the administrator.

**Vulnerability:** SQL injection in the login form.

**Idea:** Comment out the password check.

**Payload:** administrator'--

**Result:** Authentication is bypassed and the attacker logs in as admin.

---
