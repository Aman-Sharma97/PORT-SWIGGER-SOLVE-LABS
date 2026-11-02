# Two-Factor Authentication — Broken Logic

This lab's 2FA implementation has a flawed logic flow that can be exploited.

**Goal:** Reach Carlos's account page.

Your credentials: wiener:peter
Victim's username: carlos
Access to the email server is also available, to receive your own 2FA code.

Hint: Carlos never logs into the site himself.

---

**Reference:**
- https://portswigger.net/web-security/authentication/multi-factor

pic 1

---

A security code sent via email is required to complete login:

pic 2

After the first login step, a redirect sets a cookie with the value `verify=wiener`:

pic 3

Changing that value to `verify=carlos` is possible:

pic 4

With that cookie set, reaching `/login2` now shows the verification step for carlos — but no MFA code is actually sent to us, since it's his email that would receive it:

pic 5

This request was sent to Intruder:

pic 6

A brute-force attack was set up using a payload like this:

pic 7

The code "1985" triggers a different response from the rest:

pic 8

Submitting it successfully logs in as carlos:

pic 9
