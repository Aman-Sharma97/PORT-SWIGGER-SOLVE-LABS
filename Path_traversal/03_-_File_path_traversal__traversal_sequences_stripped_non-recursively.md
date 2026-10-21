# File Path Traversal — Non-Recursive Stripping of Traversal Sequences

This exercise covers a path traversal vulnerability found in how the application displays product images.

The server strips out traversal sequences from the filename before it's used, but only does a single pass — it doesn't check the result again afterward. That means a crafted sequence that still contains `../` once the stripped portion is removed will slip through.

**Goal:** Retrieve the contents of `/etc/passwd`.

---

**References:**
- https://portswigger.net/kb/issues/00100300_file-path-traversal
- https://portswigger.net/web-security/file-path-traversal

---

Lab URL generated: https://0a2a0030049b8f43822a9e64007d00ba.web-security-academy.net/

Browsing the home page or an individual post sends GET requests like this:

<img width="664" height="258" alt="1" src="https://github.com/user-attachments/assets/9288e3d0-3fa6-450b-a98c-4010d9c3ea4b" />


<img width="750" height="169" alt="2" src="https://github.com/user-attachments/assets/c5ccd0ce-f5e9-471f-9e5a-28a626ed355c" />


```
GET /image?filename=..././..././..././..././..././etc/passwd HTTP/2
```

<img width="1756" height="755" alt="3" src="https://github.com/user-attachments/assets/a35b2c7a-6dd0-455f-a34d-da88bdbe0d07" />

