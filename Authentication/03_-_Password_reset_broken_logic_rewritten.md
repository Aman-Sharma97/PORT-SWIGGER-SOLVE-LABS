# Password Reset — Broken Logic

This lab's password reset feature has a logic flaw.

**Goal:** Reset Carlos's password, then log in and view his "My account" page.

- Your credentials: wiener:peter
- Victim's username: carlos

---

**Reference:**
- https://portswigger.net/web-security/authentication/other-mechanisms

<img width="771" height="594" alt="1" src="https://github.com/user-attachments/assets/a968d846-2565-4342-b257-992db2f0b736" />


---

The login page includes a "Forgot password" link:

<img width="713" height="184" alt="2" src="https://github.com/user-attachments/assets/c15d88c8-b340-4c31-87f1-6d51a5f693f6" />


Submitting it delivers a message containing a password-reset link:

<img width="1001" height="208" alt="3" src="https://github.com/user-attachments/assets/2eb9fdcb-9bdb-493b-9a1d-fc6f4a364dde" />


Following that link, a new password can be entered:

<img width="717" height="262" alt="4" src="https://github.com/user-attachments/assets/e6ee0316-ed24-4c61-b717-1fb6987d6f97" />


This generates the following POST request:

<img width="575" height="457" alt="5" src="https://github.com/user-attachments/assets/135b7c0d-1f58-40b3-9f68-917c7d33143c" />


Swapping the `username` parameter for "carlos" lets the attacker reset his password and log into his account with it:

```
POST /forgot-password?temp-forgot-password-token=eeiyUnDFWhZqlaBJz707cipGPxh7bN4T HTTP/2
...

temp-forgot-password-token=eeiyUnDFWhZqlaBJz707cipGPxh7bN4T&username=carlos&new-password-1=password&new-password-2=password
```
