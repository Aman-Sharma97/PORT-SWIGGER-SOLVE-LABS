# Bypassing URL-Based Access Control

This site hosts an unauthenticated admin panel at `/admin`, which a front-end layer is configured to block external access to. However, the back-end framework honors the `X-Original-URL` header, which can be abused to route around that block.

**Goal:** Reach the admin panel and delete the user `carlos`.

---

**Reference:**
- https://portswigger.net/web-security/access-control

pic 1

---

Requesting `/admin` directly returns a 403:

pic 2

`/admin` can still be reached using:

```
GET / HTTP/2
...
X-Original-Url: /admin/
```

pic 3

The user can then be deleted with this request. Note that the `username` parameter must be placed in the URL itself:

```
GET /?username=carlos HTTP/2
X-Original-Url: /admin/delete
```

pic 4
