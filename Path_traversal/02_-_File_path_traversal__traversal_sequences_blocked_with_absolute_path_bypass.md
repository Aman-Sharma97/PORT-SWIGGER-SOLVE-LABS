# File Path Traversal — Bypassing a Block via Absolute Paths

This exercise covers a path traversal vulnerability found in how the application displays product images.

Here, traversal sequences (like `../`) are filtered out, but the app still treats whatever filename it receives as relative to a fixed base directory — which opens the door to simply supplying an absolute path instead.

**Goal:** Retrieve the contents of `/etc/passwd`.

---

**Reference:**
- https://portswigger.net/web-security/file-path-traversal

<img width="757" height="222" alt="1" src="https://github.com/user-attachments/assets/320e692d-0c58-41a5-a2ef-43fa80c79ab1" />


---

Images are fetched using a GET request that takes a `filename` parameter:

<img width="889" height="302" alt="2" src="https://github.com/user-attachments/assets/bccae126-ee5e-4415-86b4-813dbd49c154" />


To pull `/etc/passwd`:

```
GET /image?filename=/etc/passwd 
```

<img width="1032" height="308" alt="3" src="https://github.com/user-attachments/assets/aaf5aafa-3449-41cd-8604-808c385b892b" />

