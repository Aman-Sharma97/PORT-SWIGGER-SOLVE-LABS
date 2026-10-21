# OS Command Injection — Basic Scenario

This exercise demonstrates an OS command injection flaw present in the stock-checking feature of the application.

The backend builds a shell command using values supplied by the user (product and store identifiers), runs it, and then sends the command's raw output straight back in the HTTP response.

**Goal:** Run the `whoami` command to find out which user account the application is operating under.

---

**Reference:**
- https://portswigger.net/web-security/os-command-injection

<img width="773" height="894" alt="1" src="https://github.com/user-attachments/assets/e07f6ec3-d96f-4d85-9cab-8dec1a934e3a" />


---

<img width="469" height="439" alt="2" src="https://github.com/user-attachments/assets/f9123f5b-2f7f-4586-9efa-b0e898a45e99" />


<img width="946" height="135" alt="3" src="https://github.com/user-attachments/assets/e0c0450c-7931-42e0-bbab-70f2068e652e" />


The vulnerable request/payload used:

```
POST /product/stock HTTP/2
...

productId=1&storeId=1;whoami
```

<img width="941" height="429" alt="4" src="https://github.com/user-attachments/assets/daaa7d80-d358-4384-8799-9cf240391fa9" />

