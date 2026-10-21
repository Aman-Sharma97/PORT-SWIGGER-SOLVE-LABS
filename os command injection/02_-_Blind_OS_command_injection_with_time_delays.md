# Blind OS Command Injection — Using Time Delays

This exercise involves a blind OS command injection issue located in the feedback submission feature.

Here, the server constructs a shell command from details entered by the user, but the command's output is never shown back in the response — making it a "blind" vulnerability.

**Goal:** Trigger the blind command injection so that the server's response is delayed by 10 seconds.

---

**Reference:**
- https://portswigger.net/web-security/os-command-injection

<img width="768" height="298" alt="1" src="https://github.com/user-attachments/assets/0813b5bf-1aa6-45e3-b520-a7e532f870c9" />


---

The app includes a feedback form:

<img width="723" height="630" alt="2" src="https://github.com/user-attachments/assets/e42dc6aa-b915-4d65-a6ff-7c0034a659eb" />


The injectable fields are "Name", "Email", and "Message". The payload used:

```
"; ping -c 127.0.0.1; echo "a
```

<img width="722" height="630" alt="3" src="https://github.com/user-attachments/assets/7e2852dc-20aa-4021-a4cc-ea06a96753b8" />

