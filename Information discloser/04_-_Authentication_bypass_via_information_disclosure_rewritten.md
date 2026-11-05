# Authentication Bypass via Information Disclosure

The admin interface in this lab has an authentication bypass flaw, but exploiting it requires knowing the name of a custom HTTP header used by the front-end — without that, it isn't practically exploitable.

**Goal:** Discover that header name, use it to bypass authentication, reach the admin interface, and delete Carlos's account.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/information-disclosure/exploiting

pic 1

---

After logging in, sending a request with the `TRACE` HTTP method reveals the header name: `X-Custom-IP-Authorization`:

pic 2

`/admin` can then be reached using:

```
GET /admin HTTP/2
...
X-Custom-Ip-Authorization: 127.0.0.1
```

pic 3

And the target user deleted with:

```
GET /admin/delete?username=carlos HTTP/2
...
X-Custom-Ip-Authorization: 127.0.0.1
```

pic 4
