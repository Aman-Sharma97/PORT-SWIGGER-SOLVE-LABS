# Host Validation Bypass via Connection State Attack

This lab is vulnerable to routing-based SSRF through the `Host` header. While the front-end server appears to validate the `Host` header carefully at first glance, it actually bases its assumptions about every request on a connection on just the first request it receives.

**Goal:** Exploit this behavior to reach an internal admin panel at `192.168.0.1/admin`, then delete the user `carlos`.

Note: Solving this lab requires features introduced in Burp Suite 2022.8.1.

This lab is modeled on real-world vulnerabilities uncovered by PortSwigger Research — see "Browser-Powered Desync Attacks: A New Frontier in HTTP Request Smuggling" for background.

---

**References:**
- https://portswigger.net/web-security/host-header/exploiting
- https://portswigger.net/research/browser-powered-desync-attacks#state

<img width="768" height="416" alt="1" src="https://github.com/user-attachments/assets/0ad3c839-5671-481b-9a87-7321d3cc8327" />

<img width="868" height="536" alt="2" src="https://github.com/user-attachments/assets/985571b2-b8e5-491e-94d0-f3c3d75584d2" />


---

First, a request is sent to the home page using the legitimate `Host` header:

<img width="666" height="496" alt="3" src="https://github.com/user-attachments/assets/1cd2144b-7518-49b8-ab0d-564d7f2b0709" />


Then, immediately afterward, a request to `/admin` is sent with the `Host` header swapped for `192.168.0.1`:

<img width="695" height="477" alt="4" src="https://github.com/user-attachments/assets/a37c725d-93b1-4766-be52-e0f1606c69ea" />


Following a redirect, the admin panel becomes accessible, revealing the `csrf` value:

<img width="892" height="576" alt="5" src="https://github.com/user-attachments/assets/a3e1eb20-60ba-4803-8376-f071bf8b901a" />


After sending a request to `/` with the legitimate `Host` header restored, the deletion request can then be sent:

```
GET / HTTP/2
Host: 0a6a005e030d906b874c2bf300490073.web-security-academy.net
...
```

```
POST /admin/delete HTTP/2
Host: 192.168.0.1
...

username=carlos&csrf=CY6qJXGEMwYMKu0z18mEKfFo2zAoE866
```

<img width="727" height="441" alt="6" src="https://github.com/user-attachments/assets/8e830ed5-47ed-441c-a591-381cc276d8b5" />

