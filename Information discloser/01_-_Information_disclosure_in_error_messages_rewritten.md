# Information Disclosure via Error Messages

This lab's overly detailed error output reveals that it's running a vulnerable version of a third-party framework.

**Goal:** Find and submit that framework's version number.

---

**Reference:**
- https://portswigger.net/web-security/information-disclosure/exploiting

pic 1

---

To provoke an error, I sent a string value where an integer was expected:

```
GET /product?productId=aaa
```

pic 2
