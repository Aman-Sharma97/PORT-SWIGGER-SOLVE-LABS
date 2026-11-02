# Referer-Based Access Control

This lab gates certain admin functions based on the `Referer` header. The admin panel can first be explored by logging in with administrator:admin.

**Goal:** Log in as wiener:peter and exploit the flawed access control to promote your own account to administrator.

---

**Reference:**
- https://portswigger.net/web-security/access-control

<img width="762" height="310" alt="1" src="https://github.com/user-attachments/assets/d5e6f808-5892-4421-89c9-ad0d17f1a92f" />


---

The user-upgrade action is triggered from the admin panel via a GET request:

<img width="722" height="380" alt="2" src="https://github.com/user-attachments/assets/0f36083b-d4a9-43b3-a3c4-2f844d31cef8" />


Attempting it as "wiener" directly returns an unauthorized error:

<img width="755" height="407" alt="3" src="https://github.com/user-attachments/assets/4e23c164-0b0c-4d2f-b24e-9214600f233d" />


Setting the `Referer` header to point at `/admin` makes it succeed:

```
GET /admin-roles?username=wiener&action=upgrade HTTP/2
...
Referer: https://0afd00fc039e68c881769dfa009b00b8.web-security-academy.net/admin
...
```

<img width="724" height="371" alt="4" src="https://github.com/user-attachments/assets/4cb6267b-9169-4cfd-b577-326659140ea6" />

