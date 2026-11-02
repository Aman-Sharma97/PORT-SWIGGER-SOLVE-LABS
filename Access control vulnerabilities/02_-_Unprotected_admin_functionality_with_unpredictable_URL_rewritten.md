# Unprotected Admin Functionality at an Unguessable URL

This lab's admin panel has no access controls, but sits at a path that's hard to guess — though that path is leaked somewhere else in the app.

**Goal:** Locate the admin panel and use it to delete the user `carlos`.

---

**Reference:**
- https://portswigger.net/web-security/access-control

pic 1

---

Viewing the page source (Ctrl+U) reveals the admin panel's location:

pic 2

From there, the user can be deleted:

pic 3
