# Insecure Direct Object References

This lab saves users' chat logs as plain files on the server and serves them through predictable static URLs.

**Goal:** Find `carlos`'s password and log into his account with it.

---

**Reference:**
- https://portswigger.net/web-security/access-control

pic 1

---

There's a live chat feature:

pic 2

Its transcript can be downloaded:

pic 3

Swapping the filename for `1.txt` reveals the password:

pic 4
