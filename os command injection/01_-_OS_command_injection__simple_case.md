# OS Command Injection — Basic Scenario

This exercise demonstrates an OS command injection flaw present in the stock-checking feature of the application.

The backend builds a shell command using values supplied by the user (product and store identifiers), runs it, and then sends the command's raw output straight back in the HTTP response.

**Goal:** Run the `whoami` command to find out which user account the application is operating under.

---

**Reference:**
- https://portswigger.net/web-security/os-command-injection

pic 1

---

pic 2

pic 3

The vulnerable request/payload used:

```
POST /product/stock HTTP/2
...

productId=1&storeId=1;whoami
```

pic 4
