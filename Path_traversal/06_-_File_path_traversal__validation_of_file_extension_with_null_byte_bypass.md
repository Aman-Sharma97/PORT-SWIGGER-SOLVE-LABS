# File Path Traversal — Bypassing Extension Checks with a Null Byte

This exercise covers a path traversal vulnerability found in how the application displays product images.

The app checks that the supplied filename ends in the expected extension (e.g., `.png`) before serving it — but this check can be defeated by appending a null byte after the traversal path, which causes the underlying file system call to ignore everything after it.

**Goal:** Retrieve the contents of `/etc/passwd`.

---

**Reference:**
- https://portswigger.net/web-security/file-path-traversal

<img width="761" height="229" alt="1" src="https://github.com/user-attachments/assets/61543233-de84-4302-ac4d-ef2a06d206bf" />


---

Images are fetched using a GET request that takes a `filename` parameter:

<img width="1031" height="310" alt="2" src="https://github.com/user-attachments/assets/76aa01b1-299c-4ea3-9b0e-cba8ca3a8813" />


To pull `/etc/passwd`, a null byte is used to truncate the extension check:

```
GET /image?filename=../../../etc/passwd%00.png
```

<img width="1032" height="307" alt="3" src="https://github.com/user-attachments/assets/1a88fac5-844d-478d-8e78-b72485e7089f" />

