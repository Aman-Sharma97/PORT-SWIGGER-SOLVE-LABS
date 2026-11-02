# Unprotected Admin Functionality

This lab exposes an admin panel with no access restrictions at all.

**Goal:** Delete the user `carlos`.

---

**Reference:**
- https://portswigger.net/web-security/access-control

<img width="760" height="498" alt="1" src="https://github.com/user-attachments/assets/264266bf-2020-44c3-9bf1-d1519bc6d75f" />


---

The admin panel's path is disclosed in `robots.txt`:

<img width="613" height="134" alt="2" src="https://github.com/user-attachments/assets/4256d90b-4c33-402d-8b2c-d974b17293ee" />


From there, the user can be deleted directly:

<img width="681" height="382" alt="3" src="https://github.com/user-attachments/assets/827e67cb-156e-4bf3-88a3-9f063dcc8b99" />

