
# Stored XSS into anchor href attribute with double quotes HTML-encoded

The comment feature stores submitted content and later displays it on the blog post, making this a stored XSS scenario. To solve this lab, submit a comment that calls the alert function when the comment author name is clicked.


---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/contexts



**Pic 1**

---------------------------------------------

The blog post provides a comment form, allowing content to be submitted:



**Pic 2**

This is the HTML element generated:



**Pic 3**


When the user name is clicked, it redirects to the website set in the comment:





**Pic 4**
**Pic 5**

We will set the website to a javascript url:

```
javascript:alert(1)
```




**Pic 6**


When clicked, the alert pops:



**Pic 7**
