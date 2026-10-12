
# Reflected XSS into a JavaScript string with angle brackets HTML encoded

This lab contains a reflected cross-site scripting vulnerability in the search query tracking functionality where angle brackets are encoded. The reflection occurs inside a JavaScript string. To solve this lab, perform a cross-site scripting attack that breaks out of the JavaScript string and calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/contexts




**Pic 1**

---------------------------------------------


Searching for `aaaa` produces a page containing the following code:



**Pic 2**

Start by testing a payload such as:

```
';alert(1);echo 'a
```

We see the code is now:



**Pic 3**

This payload causes the alert to appear:

```
';alert(1)//
```



**Pic 4**
