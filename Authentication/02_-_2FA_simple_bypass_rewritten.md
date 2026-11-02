# Simple Two-Factor Authentication Bypass

This lab's 2FA can be sidestepped entirely. A valid username and password are already available, but the user's 2FA code is not.

**Goal:** Reach Carlos's account page.

- Your credentials: wiener:peter
- Victim's credentials: carlos:montoya

---

**Reference:**
- https://portswigger.net/web-security/authentication/multi-factor

pic 1

---

After authenticating, two pages are reachable: `/` and `/my-account?id=wiener`:

pic 2

Logging in as carlos still requires submitting a security code:

pic 3

Simply navigating to `/` and clicking "Home" bypasses that step entirely:

pic 4
