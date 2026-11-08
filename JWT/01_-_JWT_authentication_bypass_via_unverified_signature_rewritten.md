# JWT Authentication Bypass — Unverified Signature

This lab uses JWTs to manage sessions. Due to an implementation flaw, the server never actually checks the signature on any JWT it receives.

**Goal:** Modify your session token to gain access to the admin panel at `/admin`, then delete the user `carlos`.

Login credentials for your own account: wiener:peter

Tip: Getting comfortable with handling JWTs in Burp Suite beforehand is recommended.

---

**Reference:**
- https://portswigger.net/web-security/jwt

<img width="765" height="667" alt="1" src="https://github.com/user-attachments/assets/8d4f3428-a80d-424c-8456-2eeb4305ab81" />


---

After logging in, a JWT is issued:

<img width="511" height="274" alt="2" src="https://github.com/user-attachments/assets/2675d006-0d52-46ee-99de-bc439eab11e6" />


This token can be edited directly in Burp's JSON Web Token extension panel:

<img width="868" height="611" alt="3" src="https://github.com/user-attachments/assets/a4df663e-70d1-4c65-bf74-e91db9768f93" />


The issued JWT:

```
eyJraWQiOiI0MTZkMDg2Yy00MDdhLTRiYzQtODhhMy00MzAyZTUzMTk1ZTgiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3ODg2MzR9.qmaz_uqHRR06JTx5vtenCveTPOtzi3mG0X1WMJhnKV3AmzlI3Pjceo3Lldu2oLHcP9SEblyJxJJ5hIO3VVAKzWsWGjNw4aN1vZCBhxzcY-MgxuspBc3XpS1_oMeenFcfEn0I4Jlob_YMrZVqQbdp8i1w_SpYLkMOkDaLlgPZk3TwZa1U005YBhHjQrItMBYWRtQDnP4rYnHkTsgwmWRu8RMCirq9-SS9gczbr2YEENZuPrxWphbYwCSMtivcysFOKXEzCvO7juIKqAfE_WmB6qx41I8Wny-qlkbeU3-9VXyIM8iC6opD6wlUiI9S328bjXN_ZFWsuRdaDVyvE4gRXw
```

I then set the `Location` header's value to `/admin`:

<img width="464" height="301" alt="4" src="https://github.com/user-attachments/assets/cd42371d-4a94-4aa5-8e16-3f74f4967420" />


Admin panel access is granted:

<img width="485" height="166" alt="5" src="https://github.com/user-attachments/assets/0092ded5-4c80-40c5-93f2-f8725e3dbc81" />

