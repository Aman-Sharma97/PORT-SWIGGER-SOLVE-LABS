# Password Reset — Broken Logic

This lab's password reset feature has a logic flaw.

**Goal:** Reset Carlos's password, then log in and view his "My account" page.

- Your credentials: wiener:peter
- Victim's username: carlos

---

**Reference:**
- https://portswigger.net/web-security/authentication/other-mechanisms

pic 1

---

The login page includes a "Forgot password" link:

pic 2

Submitting it delivers a message containing a password-reset link:

pic 3

Following that link, a new password can be entered:

pic 4

This generates the following POST request:

pic 5

Swapping the `username` parameter for "carlos" lets the attacker reset his password and log into his account with it:

```
POST /forgot-password?temp-forgot-password-token=eeiyUnDFWhZqlaBJz707cipGPxh7bN4T HTTP/2
...

temp-forgot-password-token=eeiyUnDFWhZqlaBJz707cipGPxh7bN4T&username=carlos&new-password-1=password&new-password-2=password
```
