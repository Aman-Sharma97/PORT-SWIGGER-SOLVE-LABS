# File Path Traversal — Extra URL-Decode Step After Sanitization

This exercise covers a path traversal vulnerability found in how the application displays product images.

The app rejects any input containing traversal sequences, but then goes on to URL-decode that input afterward before actually using it — meaning a payload that's encoded once won't get flagged by the filter, but will still resolve to a traversal sequence once decoded.

**Goal:** Retrieve the contents of `/etc/passwd`.

---

**Reference:**
- https://portswigger.net/web-security/file-path-traversal

pic 1

---

Images are fetched using a GET request that takes a `filename` parameter:

pic 2

To pull `/etc/passwd`, the traversal characters need to be double URL-encoded:

```
GET /image?filename=%252e%252e%252f%252e%252e%252f%252e%252e%252fetc/passwd
```

pic 3
