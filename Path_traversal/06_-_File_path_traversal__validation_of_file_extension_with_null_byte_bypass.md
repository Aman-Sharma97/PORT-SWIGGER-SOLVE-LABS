# File Path Traversal — Bypassing Extension Checks with a Null Byte

This exercise covers a path traversal vulnerability found in how the application displays product images.

The app checks that the supplied filename ends in the expected extension (e.g., `.png`) before serving it — but this check can be defeated by appending a null byte after the traversal path, which causes the underlying file system call to ignore everything after it.

**Goal:** Retrieve the contents of `/etc/passwd`.

---

**Reference:**
- https://portswigger.net/web-security/file-path-traversal

pic 1

---

Images are fetched using a GET request that takes a `filename` parameter:

pic 2

To pull `/etc/passwd`, a null byte is used to truncate the extension check:

```
GET /image?filename=../../../etc/passwd%00.png
```

pic 3
