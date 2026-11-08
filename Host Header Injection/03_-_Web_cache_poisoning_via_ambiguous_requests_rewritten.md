# Web Cache Poisoning via Ambiguous Requests

This lab's cache and back-end disagree on how to interpret ambiguous requests, opening it up to cache poisoning. A victim user regularly browses the site's home page.

**Goal:** Poison the cache so that the home page runs `alert(document.cookie)` in the victim's browser.

---

**Reference:**
- https://portswigger.net/web-security/host-header/exploiting

pic 1

---

The cache keys couldn't be identified using `Pragma: x-get-cache-key`, but the response headers reveal the cache entry lasts 30 seconds.

The `Host` header gets reflected in the response, but supplying just one arbitrary value triggers a gateway error. The trick is to send two `Host` headers — the first (malicious) one gets reflected, while the second (legitimate) one keeps the request valid:

pic 2

JavaScript can be injected using a payload like this:

```
GET / HTTP/2
Host: testing123"></script><script>alert(document.cookie)</script><script>
Host: 0af500ba04a9e93b802c499200750009.web-security-academy.net
...
```

pic 3

The alert box pops showing the cookie value:

pic 4
