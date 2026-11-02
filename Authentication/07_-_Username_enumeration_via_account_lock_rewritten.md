# Username Enumeration via Account Lockout

This lab is vulnerable to username enumeration. It locks accounts after repeated failures, but that mechanism has a logic flaw.

**Goal:** Enumerate a valid username, brute-force that user's password, then log in and view their account page.

- Candidate usernames
- Candidate passwords

---

**Reference:**
- https://portswigger.net/web-security/authentication/password-based

pic 1

---

The login's POST request was intercepted and sent to Intruder:

pic 2

The user "pi" ends up locked:

pic 3

With that username fixed, every candidate password is then tested:

pic 4

One request stands out — rather than an "invalid password" or "too many attempts" message, it returns a blank response, which turns out to mark the correct password:

pic 5
