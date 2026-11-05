# Information Disclosure via Error Messages

This lab's overly detailed error output reveals that it's running a vulnerable version of a third-party framework.

**Goal:** Find and submit that framework's version number.

---

**Reference:**
- https://portswigger.net/web-security/information-disclosure/exploiting

<img width="768" height="501" alt="1" src="https://github.com/user-attachments/assets/257e5ee8-feb0-4748-a564-5d37c493659e" />


---

To provoke an error, I sent a string value where an integer was expected:

```
GET /product?productId=aaa
```

<img width="1033" height="731" alt="2" src="https://github.com/user-attachments/assets/c022a515-52df-4c49-800c-f387e5e552a6" />

