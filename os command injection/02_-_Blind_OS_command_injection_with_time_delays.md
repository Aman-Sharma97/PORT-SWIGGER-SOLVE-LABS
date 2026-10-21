# Blind OS Command Injection — Using Time Delays

This exercise involves a blind OS command injection issue located in the feedback submission feature.

Here, the server constructs a shell command from details entered by the user, but the command's output is never shown back in the response — making it a "blind" vulnerability.

**Goal:** Trigger the blind command injection so that the server's response is delayed by 10 seconds.

---

**Reference:**
- https://portswigger.net/web-security/os-command-injection

pic 1

---

The app includes a feedback form:

pic 2

The injectable fields are "Name", "Email", and "Message". The payload used:

```
"; ping -c 127.0.0.1; echo "a
```

pic 3
