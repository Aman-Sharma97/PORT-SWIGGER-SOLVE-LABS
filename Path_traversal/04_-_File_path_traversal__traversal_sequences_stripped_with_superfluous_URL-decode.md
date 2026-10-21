# File Path Traversal — Extra URL-Decode Step After Sanitization

This exercise covers a path traversal vulnerability found in how the application displays product images.

The app rejects any input containing traversal sequences, but then goes on to URL-decode that input afterward before actually using it — meaning a payload that's encoded once won't get flagged by the filter, but will still resolve to a traversal sequence once decoded.

**Goal:** Retrieve the contents of `/etc/passwd`.

---

**Reference:**
- https://portswigger.net/web-security/file-path-traversal

<img width="767" height="291" alt="1" src="https://github.com/user-attachments/assets/5af2f5d0-23d4-413d-ba4d-46368838c3af" />


---

Images are fetched using a GET request that takes a `filename` parameter:

<img width="789" height="370" alt="2" src="https://github.com/user-attachments/assets/f85cee60-c844-488c-a228-c7bf58f9404a" />


To pull `/etc/passwd`, the traversal characters need to be double URL-encoded:

```
GET /image?filename=%252e%252e%252f%252e%252e%252f%252e%252e%252fetc/passwd
```

<img width="791" height="400" alt="3" src="https://github.com/user-attachments/assets/2cfff111-4221-411f-9be4-cf2ba52123a7" />

