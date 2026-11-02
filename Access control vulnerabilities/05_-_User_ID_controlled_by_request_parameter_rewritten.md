# User ID Controlled by a Request Parameter

The account page in this lab has a horizontal privilege escalation flaw.

**Goal:** Retrieve `carlos`'s API key and submit it as the solution.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/access-control

pic 1

---

Clicking "Home" sends this GET request:

pic 2

Swapping the `id` value for `carlos` exposes his API key:

pic 3
