# File Path Traversal — Checking Only the Beginning of the Path

This exercise covers a path traversal vulnerability found in how the application displays product images.

The application accepts the entire file path as a request parameter and only checks that it starts with the expected base folder — without verifying there's no traversal further along in the string.

**Goal:** Retrieve the contents of `/etc/passwd`.

---

**Reference:**
- https://portswigger.net/web-security/file-path-traversal

<img width="760" height="227" alt="1" src="https://github.com/user-attachments/assets/a2ebac1f-9715-4111-97ac-1a68c764f60f" />


---

Images are fetched using a GET request where the `filename` parameter holds the complete path:

<img width="1029" height="307" alt="2" src="https://github.com/user-attachments/assets/1247bc0a-161b-4ba2-9096-019a0bbcdd89" />


Since the path must begin with `/var/www/images/`, we use:

```
GET /image?filename=/var/www/images/../../../etc/passwd 
```

<img width="1032" height="326" alt="3" src="https://github.com/user-attachments/assets/ef4796c3-abc8-4be3-968d-52358e2ef5e9" />

