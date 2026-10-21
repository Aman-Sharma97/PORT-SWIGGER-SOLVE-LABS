# Blind OS Command Injection — Redirecting Output to a File

This exercise covers a blind OS command injection bug in the feedback feature.

As before, user-supplied text gets inserted into a shell command that the server executes, and the result isn't included in the HTTP response. However, since there's a world-writable directory at `/var/www/images/`, the command's output can be redirected there instead.

Because the app serves product catalog images directly from that folder, a file written there can later be fetched through the normal image-loading URL — effectively leaking the command output.

**Goal:** Run `whoami` and retrieve its output by abusing this redirection trick.

---

**Reference:** https://portswigger.net/web-security/os-command-injection

pic 1

---

Lab URL generated: https://0a19003c045b2b62809bd0800086001d.web-security-academy.net/

The feedback submission form:

pic 2

Images are loaded via a GET request:

pic 3

The command we need to execute:

```
whoami > /var/www/images/whoami.txt
```

The POST request was sent to Burp Intruder, with three fields marked as attack positions in Sniper mode:

pic 4

Three payloads were then configured:

pic 5

Putting the payload into the "subject" field causes the site to throw an error:

pic 6

After the Intruder run, a GET request to `/image?filename=whoami.txt` reveals the username `peter-5fYwD0`, confirming the attack succeeded:

pic 7

pic 8
