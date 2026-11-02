# User Role Can Be Changed via the Profile Update Endpoint

This lab's admin panel at `/admin` is restricted to logged-in users holding a `roleid` of 2.

**Goal:** Reach the admin panel and use it to delete the user `carlos`.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/access-control

pic 1

---

Logging in happens via a POST request:

pic 2

It responds with a 302 redirect:

pic 3

There's an email-update function that also uses POST:

pic 4

It returns the account's personal details:

pic 5

Adding a `roleid` field to that same POST request changes the user's role:

```
POST /my-account/change-email HTTP/2
...

{"email":"test@test.com", "roleid":2}
```

pic 6

The role gets updated:

pic 7

From there, the target user can be deleted:

pic 8
