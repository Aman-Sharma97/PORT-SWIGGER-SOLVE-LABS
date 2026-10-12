
# Reflected XSS in canonical link tag

This lab demonstrates reflected XSS in a canonical link tag. The application encodes angle brackets, but user-controlled input can still affect the tag's attributes.

The objective is to inject an attribute through the home page that invokes the `alert` function.

For this lab, assume the simulated user will press one of these key combinations:

ALT+SHIFT+X
CTRL+ALT+X
Alt+X

Important: the intended approach for this lab works only in Chrome.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/contexts

- https://portswigger.net/research/xss-in-hidden-input-fields








<img width="849" height="188" alt="1" src="https://github.com/user-attachments/assets/d06bb203-0c32-44e0-b442-6884b01b2231" />

<img width="956" height="141" alt="2" src="https://github.com/user-attachments/assets/54318092-0953-4c5e-848f-a5bae5e8a1bb" />

<img width="951" height="172" alt="3" src="https://github.com/user-attachments/assets/52c0de94-a814-4643-9394-9b0e76ed41ea" />


---------------------------------------------

The page includes a comment-posting feature, as shown below:



<img width="719" height="592" alt="4" src="https://github.com/user-attachments/assets/6f6609d3-0ff1-4c1d-ab33-ed3034b76f70" />



Inspecting the page's HTML head reveals a link element with `rel="canonical"`:



<img width="822" height="92" alt="5" src="https://github.com/user-attachments/assets/c7f4e91b-f419-450c-bb81-3663a2e2332d" />



The goal is to make that element resemble the following markup:

```
<link rel="canonical" accesskey="X" onclick="alert(1)" />
```

The `/post` endpoint still requires a valid `postId`. Additional query parameters can also influence the generated `href` attribute, as illustrated below:



<img width="674" height="446" alt="6" src="https://github.com/user-attachments/assets/90f582dd-d104-4392-8820-2c6ff3ea1a40" />



The payload used for this lab is:

```
/post?postId=1&a=b'accesskey='X'onclick='alert(1)
```






<img width="461" height="186" alt="7" src="https://github.com/user-attachments/assets/aeaa319c-1e5a-426b-b0a9-39baada241c8" />

<img width="585" height="169" alt="8" src="https://github.com/user-attachments/assets/00baa6f5-caf2-48a1-a64c-1b29a2e279c2" />

