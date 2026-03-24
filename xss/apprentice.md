# Cross-Site Scripting (XSS) Labs  PortSwigger

## Apprentice

### 1. Reflected XSS in HTML context (no encoding)

**Goal:** Execute JavaScript via the search function.

**Vulnerability:** User input is reflected directly into HTML without encoding.

**Payload**
<script>alert(1)</script>

**Result:** The script executes when the search results page loads.

---

### 2. Stored XSS in HTML context

**Goal:** Execute JavaScript when users view a blog comment.

**Vulnerability:** User input is stored and rendered without sanitization.

**Payload**
<script>alert(1)</script>

**Result:** The script runs whenever the blog post is viewed.

---

### 3. DOM XSS via document.write using location.search

**Goal:** Execute JavaScript through DOM manipulation.

**Vulnerability:** User input from `location.search` is written to the page using `document.write`.

**Payload**

"><svg onload=alert(1)>


**Result:** Breaks out of the `img` attribute and executes JavaScript.

---

### 4. DOM XSS via innerHTML

**Goal:** Trigger JavaScript execution via DOM injection.

**Vulnerability:** Input is inserted into the page using `innerHTML`.

**Payload**
<img src=1 onerror=alert(1)> ```

**Result:** The invalid image triggers the onerror event.

---

### 5. DOM XSS in jQuery href attribute

**Goal:** Execute JavaScript via a manipulated link.

**Vulnerability:** location.search controls an anchor's href.

**Payload:** javascript:alert(document.cookie)

**Result:** Clicking the Back link executes the payload.

---

### 6. Stored XSS in anchor href (double quotes encoded)

**Goal:** Execute JavaScript when clicking the comment author's name.

**Vulnerability:** Website input is reflected inside an anchor tag.

**Payload:** javascript:alert(1)

**Result:** Clicking the username triggers the script.

---

### 7. Reflected XSS inside JavaScript string

**Goal:** Escape a JavaScript string and execute code.

**Vulnerability:** Input reflected inside a JavaScript string.

**Payload:** '-alert(1)-'

**Result:** Breaks out of the string and executes JavaScript.
