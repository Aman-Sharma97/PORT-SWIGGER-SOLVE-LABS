# Multi-Step Process Missing Access Control on One Step

This lab's admin panel has a flawed multi-step flow for changing a user's role. The flow can first be explored by logging in with administrator:admin.

**Goal:** Log in as wiener:peter and exploit the flawed access control to promote your own account to administrator.

---

**Reference:** https://portswigger.net/web-security/access-control

---

Lab URL generated: https://0ac700520305fa43816f2532000c0090.web-security-academy.net/

Sending the POST request to upgrade the user directly:

<img width="807" height="349" alt="1" src="https://github.com/user-attachments/assets/9ed76eff-d39d-4728-aacf-5bbbdc7dbdd0" />


This returns a 401 Unauthorized error:

<img width="398" height="161" alt="2" src="https://github.com/user-attachments/assets/6a6f1189-f5a0-4732-8c89-00f4aa1058f3" />


Trying again, this time including the confirmation parameter:

<img width="809" height="356" alt="3" src="https://github.com/user-attachments/assets/68e1b948-8821-47b0-9604-b01b613cdeba" />


This produces a redirect followed by another 401 Unauthorized error:

<img width="641" height="270" alt="4" src="https://github.com/user-attachments/assets/dcff4ffc-10fe-4cc7-b50a-aac89bb3e995" />


Despite that error, the role change actually went through — the user was promoted, and wiener can now access the admin panel:

<img width="714" height="204" alt="5" src="https://github.com/user-attachments/assets/f717fc57-ea29-4b74-a6c7-c786a02b58d9" />

