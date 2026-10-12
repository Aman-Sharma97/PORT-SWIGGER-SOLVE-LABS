
# Reflected XSS into attribute with angle brackets HTML-encoded

This lab contains a reflected cross-site scripting vulnerability in the search blog functionality where angle brackets are HTML-encoded. To solve this lab, perform a cross-site scripting attack that injects an attribute and calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/contexts



**Pic 1**

---------------------------------------------

Searching for `aaaa` shows that the supplied text is placed in this HTML element's `value` attribute:



**Pic 2**

This payload causes the alert to appear:

```
" autofocus onfocus=alert(1) x="
```



**Pic 3**


This is the HTML content which explains why the payload worked:



**Pic 4**
