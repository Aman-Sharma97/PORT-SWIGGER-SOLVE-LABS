# User Role Can Be Changed via the Profile Update Endpoint

This lab's admin panel at `/admin` is restricted to logged-in users holding a `roleid` of 2.

**Goal:** Reach the admin panel and use it to delete the user `carlos`.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/access-control

<img width="768" height="431" alt="1" src="https://github.com/user-attachments/assets/9fe18651-0dab-4b53-95b1-f5bd1d523d53" />


---

Logging in happens via a POST request:

<img width="733" height="347" alt="2" src="https://github.com/user-attachments/assets/9901ea75-5488-4601-928b-8ae860d2ceca" />


It responds with a 302 redirect:

<img width="722" height="199" alt="3" src="https://github.com/user-attachments/assets/ef3bdff6-4b76-414c-87f0-34e7b6cdbd58" />


There's an email-update function that also uses POST:

<img width="727" height="348" alt="4" src="https://github.com/user-attachments/assets/b57de755-623a-4dc6-96e0-562c2e6d5ffc" />


It returns the account's personal details:

<img width="411" height="219" alt="5" src="https://github.com/user-attachments/assets/e83f616c-730a-4450-9d0f-03c2387d7f46" />


Adding a `roleid` field to that same POST request changes the user's role:

```
POST /my-account/change-email HTTP/2
...

{"email":"test@test.com", "roleid":2}
```

<img width="729" height="350" alt="6" src="https://github.com/user-attachments/assets/40f8add5-32df-4a88-acd8-d7c25968fe81" />


The role gets updated:

<img width="409" height="211" alt="7" src="https://github.com/user-attachments/assets/f4e4d45a-2e57-4458-9529-353d1a6454ce" />


From there, the target user can be deleted:

<img width="614" height="144" alt="8" src="https://github.com/user-attachments/assets/4f1a5672-8427-4cad-a579-da4b39959ff5" />

