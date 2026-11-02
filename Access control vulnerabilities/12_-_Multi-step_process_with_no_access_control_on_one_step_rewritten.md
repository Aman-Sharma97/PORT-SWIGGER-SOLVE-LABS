# Multi-Step Process Missing Access Control on One Step

This lab's admin panel has a flawed multi-step flow for changing a user's role. The flow can first be explored by logging in with administrator:admin.

**Goal:** Log in as wiener:peter and exploit the flawed access control to promote your own account to administrator.

---

**Reference:** https://portswigger.net/web-security/access-control

---

Lab URL generated: https://0ac700520305fa43816f2532000c0090.web-security-academy.net/

Sending the POST request to upgrade the user directly:

pic 1

This returns a 401 Unauthorized error:

pic 2

Trying again, this time including the confirmation parameter:

pic 3

This produces a redirect followed by another 401 Unauthorized error:

pic 4

Despite that error, the role change actually went through — the user was promoted, and wiener can now access the admin panel:

pic 5
