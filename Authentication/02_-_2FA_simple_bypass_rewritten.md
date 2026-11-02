# Simple Two-Factor Authentication Bypass

This lab's 2FA can be sidestepped entirely. A valid username and password are already available, but the user's 2FA code is not.

**Goal:** Reach Carlos's account page.

- Your credentials: wiener:peter
- Victim's credentials: carlos:montoya

---

**Reference:**
- https://portswigger.net/web-security/authentication/multi-factor

<img width="764" height="284" alt="1" src="https://github.com/user-attachments/assets/d167d713-9e52-4fc0-b37f-fb114e29f4da" />


---

After authenticating, two pages are reachable: `/` and `/my-account?id=wiener`:

<img width="718" height="313" alt="2" src="https://github.com/user-attachments/assets/7ac0f192-6a69-4926-88c5-f4c9466137d7" />


Logging in as carlos still requires submitting a security code:

<img width="719" height="196" alt="3" src="https://github.com/user-attachments/assets/f11f3d67-d6ae-4a19-a765-72917e7a15bd" />


Simply navigating to `/` and clicking "Home" bypasses that step entirely:

<img width="714" height="296" alt="4" src="https://github.com/user-attachments/assets/64d8bd3c-b422-475a-a004-849becd96677" />

