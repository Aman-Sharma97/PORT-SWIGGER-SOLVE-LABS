# User ID Controlled by a Request Parameter — Data Leaked in a Redirect

This lab's access control flaw leaks sensitive data inside the body of a redirect response.

**Goal:** Retrieve `carlos`'s API key and submit it as the solution.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/access-control

pic 1

---

Clicking "Home" sends this GET request:

pic 2

Swapping the `id` value for "carlos" triggers a redirect:

pic 3

The API key turns out to be present in that redirect response body:

pic 4
