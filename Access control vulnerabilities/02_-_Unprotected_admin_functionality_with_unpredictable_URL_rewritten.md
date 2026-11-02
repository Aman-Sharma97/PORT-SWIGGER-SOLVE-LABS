# Unprotected Admin Functionality at an Unguessable URL

This lab's admin panel has no access controls, but sits at a path that's hard to guess — though that path is leaked somewhere else in the app.

**Goal:** Locate the admin panel and use it to delete the user `carlos`.

---

**Reference:**
- https://portswigger.net/web-security/access-control

<img width="760" height="498" alt="1" src="https://github.com/user-attachments/assets/64ed06cd-4fa1-4e4e-a44f-2ebff6c33cde" />


---

Viewing the page source (Ctrl+U) reveals the admin panel's location:

<img width="522" height="168" alt="2" src="https://github.com/user-attachments/assets/c11e431a-bc0d-4524-8398-fe88b9d60e76" />


From there, the user can be deleted:

<img width="767" height="384" alt="3" src="https://github.com/user-attachments/assets/04ec3a16-477a-4469-bd50-9111cea51b8e" />

