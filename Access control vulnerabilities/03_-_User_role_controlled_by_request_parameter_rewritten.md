# User Role Controlled by a Request Parameter

This lab's admin panel at `/admin` identifies administrators using a cookie that can be freely forged.

**Goal:** Reach the admin panel and use it to delete the user `carlos`.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/access-control

pic 1

---

After logging in, a redirect sets the `Admin` cookie's value to `false`:

pic 2

Flipping that value to `true` grants visibility into the admin panel, where the user can then be deleted:

pic 3
pic 4
