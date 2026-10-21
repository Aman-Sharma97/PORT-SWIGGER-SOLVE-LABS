# File Path Traversal — Checking Only the Beginning of the Path

This exercise covers a path traversal vulnerability found in how the application displays product images.

The application accepts the entire file path as a request parameter and only checks that it starts with the expected base folder — without verifying there's no traversal further along in the string.

**Goal:** Retrieve the contents of `/etc/passwd`.

---

**Reference:**
- https://portswigger.net/web-security/file-path-traversal

pic 1

---

Images are fetched using a GET request where the `filename` parameter holds the complete path:

pic 2

Since the path must begin with `/var/www/images/`, we use:

```
GET /image?filename=/var/www/images/../../../etc/passwd 
```

pic 3
