# Bypassing Method-Based Access Control

This lab enforces access control partly based on the HTTP method of the request. The admin panel can be explored first by logging in with administrator:admin.

**Goal:** Log in as wiener:peter and exploit the flawed access control to promote your own account to administrator.

---

**Reference:**
- https://portswigger.net/web-security/access-control

pic 1

---

The admin panel includes a feature for upgrading users:

pic 2

This action is normally a POST request:

pic 3

But it turns out to work just as well as a GET request:

pic 4

Running this as wiener successfully promotes the account to administrator:

pic 5
