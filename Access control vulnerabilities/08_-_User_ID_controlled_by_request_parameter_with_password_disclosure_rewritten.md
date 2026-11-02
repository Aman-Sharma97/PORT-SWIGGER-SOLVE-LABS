# User ID Controlled by a Request Parameter — With Password Disclosure

The account page in this lab prefills the logged-in user's existing password into a masked input field.

**Goal:** Retrieve the administrator's password and use it to delete `carlos`.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/access-control

pic 1

---

Clicking "Home" sends this GET request:

pic 2

The `id` value can be swapped for "administrator":

pic 3

Switching the password field's input type to "text" reveals it in plain view:

pic 4

From there, the target user can be deleted:

pic 5
