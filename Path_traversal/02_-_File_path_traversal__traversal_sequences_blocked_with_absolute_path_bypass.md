# File Path Traversal — Bypassing a Block via Absolute Paths

This exercise covers a path traversal vulnerability found in how the application displays product images.

Here, traversal sequences (like `../`) are filtered out, but the app still treats whatever filename it receives as relative to a fixed base directory — which opens the door to simply supplying an absolute path instead.

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
GET /image?filename=/etc/passwd 
```

pic 3
