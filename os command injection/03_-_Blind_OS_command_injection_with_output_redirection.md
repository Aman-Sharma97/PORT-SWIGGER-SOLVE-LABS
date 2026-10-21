# Blind OS Command Injection — Redirecting Output to a File

This exercise covers a blind OS command injection bug in the feedback feature.

As before, user-supplied text gets inserted into a shell command that the server executes, and the result isn't included in the HTTP response. However, since there's a world-writable directory at `/var/www/images/`, the command's output can be redirected there instead.

Because the app serves product catalog images directly from that folder, a file written there can later be fetched through the normal image-loading URL — effectively leaking the command output.

**Goal:** Run `whoami` and retrieve its output by abusing this redirection trick.

---

**Reference:** https://portswigger.net/web-security/os-command-injection

<img width="861" height="292" alt="1" src="https://github.com/user-attachments/assets/887220b3-3531-40a6-bc69-4c24633d250e" />


---

Lab URL generated: https://0a19003c045b2b62809bd0800086001d.web-security-academy.net/

The feedback submission form:

<img width="720" height="611" alt="2" src="https://github.com/user-attachments/assets/89ec174e-2568-4fcf-ad4e-75b2af5dd0ef" />


Images are loaded via a GET request:

<img width="485" height="347" alt="3" src="https://github.com/user-attachments/assets/501e27a1-4029-4c7d-8b3f-8083d8d1f78d" />


The command we need to execute:

```
whoami > /var/www/images/whoami.txt
```

The POST request was sent to Burp Intruder, with three fields marked as attack positions in Sniper mode:

<img width="837" height="282" alt="4" src="https://github.com/user-attachments/assets/ce8cc9a9-ae0e-4499-a08a-499dd8b185d0" />


Three payloads were then configured:

<img width="467" height="165" alt="5" src="https://github.com/user-attachments/assets/96a0a1fc-092e-4cd4-b374-eab76d13fd34" />


Putting the payload into the "subject" field causes the site to throw an error:

<img width="392" height="157" alt="6" src="https://github.com/user-attachments/assets/262d8fa8-61dd-4467-a924-47757a2488b6" />


After the Intruder run, a GET request to `/image?filename=whoami.txt` reveals the username `peter-5fYwD0`, confirming the attack succeeded:

<img width="787" height="274" alt="7" src="https://github.com/user-attachments/assets/2731b517-0c63-4cc6-8916-d54aa09e4dd3" />


<img width="351" height="170" alt="8" src="https://github.com/user-attachments/assets/3c5cb9f3-0404-402e-8f50-12358d8719ff" />

