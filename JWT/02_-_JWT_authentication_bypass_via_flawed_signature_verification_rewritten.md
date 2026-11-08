# JWT Authentication Bypass — Flawed Signature Verification

This lab uses JWTs to manage sessions. The server is insecurely set up to accept unsigned tokens.

**Goal:** Modify your session token to gain access to the admin panel at `/admin`, then delete the user `carlos`.

Login credentials for your own account: wiener:peter

Tip: Getting comfortable with handling JWTs in Burp Suite beforehand is recommended.

---

**Reference:**
- https://portswigger.net/web-security/jwt

pic 1

---

pic 2

pic 3

Everything after the second "." character (i.e., the signature portion) is simply deleted:

```
eyJraWQiOiIzNjlmMmFjZC1hZTUwLTQ4YzctYTM2Ny04NTczYzllNTc0ZmQiLCJhbGciOiJub25lIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3ODk4MzB9.
```

pic 4

The admin panel is then reachable, and the target user can be deleted:

pic 5
