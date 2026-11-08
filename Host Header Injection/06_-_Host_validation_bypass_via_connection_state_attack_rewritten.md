# Host Validation Bypass via Connection State Attack

This lab is vulnerable to routing-based SSRF through the `Host` header. While the front-end server appears to validate the `Host` header carefully at first glance, it actually bases its assumptions about every request on a connection on just the first request it receives.

**Goal:** Exploit this behavior to reach an internal admin panel at `192.168.0.1/admin`, then delete the user `carlos`.

Note: Solving this lab requires features introduced in Burp Suite 2022.8.1.

This lab is modeled on real-world vulnerabilities uncovered by PortSwigger Research — see "Browser-Powered Desync Attacks: A New Frontier in HTTP Request Smuggling" for background.

---

**References:**
- https://portswigger.net/web-security/host-header/exploiting
- https://portswigger.net/research/browser-powered-desync-attacks#state

pic 1
pic 2

---

First, a request is sent to the home page using the legitimate `Host` header:

pic 3

Then, immediately afterward, a request to `/admin` is sent with the `Host` header swapped for `192.168.0.1`:

pic 4

Following a redirect, the admin panel becomes accessible, revealing the `csrf` value:

pic 5

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

pic 6
