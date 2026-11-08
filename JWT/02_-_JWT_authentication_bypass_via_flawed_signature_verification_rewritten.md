# JWT Authentication Bypass — Flawed Signature Verification

This lab uses JWTs to manage sessions. The server is insecurely set up to accept unsigned tokens.

**Goal:** Modify your session token to gain access to the admin panel at `/admin`, then delete the user `carlos`.

Login credentials for your own account: wiener:peter

Tip: Getting comfortable with handling JWTs in Burp Suite beforehand is recommended.

---

**Reference:**
- https://portswigger.net/web-security/jwt

<img width="764" height="568" alt="1" src="https://github.com/user-attachments/assets/f4b0dc19-a163-4456-acaa-33178b96a910" />


---

<img width="513" height="274" alt="2" src="https://github.com/user-attachments/assets/5e1db565-d65b-43ba-8094-c5b7323acfea" />


<img width="884" height="633" alt="3" src="https://github.com/user-attachments/assets/11a65dac-0c82-402f-a1cb-54f3dc7c6582" />


Everything after the second "." character (i.e., the signature portion) is simply deleted:

```
eyJraWQiOiIzNjlmMmFjZC1hZTUwLTQ4YzctYTM2Ny04NTczYzllNTc0ZmQiLCJhbGciOiJub25lIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3ODk4MzB9.
```

<img width="436" height="197" alt="4" src="https://github.com/user-attachments/assets/446e7ce9-7ae7-447d-9b10-9d3e56141f7f" />


The admin panel is then reachable, and the target user can be deleted:

<img width="711" height="344" alt="5" src="https://github.com/user-attachments/assets/18d3b5a6-4fec-4104-abbe-a2a5fae1a202" />

