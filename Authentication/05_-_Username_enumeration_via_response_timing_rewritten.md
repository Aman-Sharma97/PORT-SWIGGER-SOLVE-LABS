# Username Enumeration via Response Timing

This lab leaks valid usernames through how long its responses take. 

**Goal:** Enumerate a valid username, brute-force that user's password, then log in and view their account page.

Your credentials: wiener:peter

- Candidate usernames
- Candidate passwords

Hint: The lab also applies a form of IP-based brute-force protection, but this can be sidestepped just by manipulating HTTP headers.

---

**Reference:**
- https://portswigger.net/web-security/authentication/password-based

pic 1

---

A Pitchfork attack was configured, adding an `X-Forwarded-For` header as an extra injection point:

pic 2

Incrementing numbers were appended to the spoofed IP in that header:

pic 3

The "Columns" button at the top lets extra data be shown:

pic 4

This makes it possible to see which requests took noticeably longer — using a very long password string here is key to making the timing difference clear:

pic 5

A redirect eventually appears, giving the credentials accounts:jessica:

pic 6
