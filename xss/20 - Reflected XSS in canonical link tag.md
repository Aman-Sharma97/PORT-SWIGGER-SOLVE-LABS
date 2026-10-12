
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








**Pic 1**
**Pic 2**
**Pic 3**

---------------------------------------------

The page includes a comment-posting feature, as shown below:



**Pic 4**


Inspecting the page's HTML head reveals a link element with `rel="canonical"`:



**Pic 5**


The goal is to make that element resemble the following markup:

```
<link rel="canonical" accesskey="X" onclick="alert(1)" />
```

The `/post` endpoint still requires a valid `postId`. Additional query parameters can also influence the generated `href` attribute, as illustrated below:



**Pic 6**


The payload used for this lab is:

```
/post?postId=1&a=b'accesskey='X'onclick='alert(1)
```






**Pic 7**
**Pic 8**
