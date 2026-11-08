# Web Cache Poisoning via Ambiguous Requests

This lab's cache and back-end disagree on how to interpret ambiguous requests, opening it up to cache poisoning. A victim user regularly browses the site's home page.

**Goal:** Poison the cache so that the home page runs `alert(document.cookie)` in the victim's browser.

---

**Reference:**
- https://portswigger.net/web-security/host-header/exploiting

<img width="767" height="521" alt="1" src="https://github.com/user-attachments/assets/d9af21a3-7b8c-4e6c-a584-0a04dde7da27" />


---

The cache keys couldn't be identified using `Pragma: x-get-cache-key`, but the response headers reveal the cache entry lasts 30 seconds.

The `Host` header gets reflected in the response, but supplying just one arbitrary value triggers a gateway error. The trick is to send two `Host` headers — the first (malicious) one gets reflected, while the second (legitimate) one keeps the request valid:

<img width="906" height="445" alt="2" src="https://github.com/user-attachments/assets/6a15f268-04f4-41ba-ae24-ffd808d4fdd8" />


JavaScript can be injected using a payload like this:

```
GET / HTTP/2
Host: testing123"></script><script>alert(document.cookie)</script><script>
Host: 0af500ba04a9e93b802c499200750009.web-security-academy.net
...
```

<img width="793" height="551" alt="3" src="https://github.com/user-attachments/assets/7a7383e0-6393-4bf6-a0a4-46822451ebf2" />


The alert box pops showing the cookie value:

<img width="843" height="445" alt="4" src="https://github.com/user-attachments/assets/e7bd4a6a-fb00-49b2-a0a1-2b64fa2a3c44" />

