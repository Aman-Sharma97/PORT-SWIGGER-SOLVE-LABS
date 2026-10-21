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

pic 1

pic 2

```
GET /image?filename=..././..././..././..././..././etc/passwd HTTP/2
```

pic 3
