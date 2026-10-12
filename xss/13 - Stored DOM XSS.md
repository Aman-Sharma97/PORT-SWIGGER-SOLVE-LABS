
# Stored DOM XSS

This lab demonstrates a stored DOM vulnerability in the blog comment functionality. To solve this lab, exploit this vulnerability to call the alert() function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------

The blog post provides a comment form, allowing content to be submitted:



**Pic 1**


After submission, the comment is represented by the following HTML:



**Pic 2**

We can try the payload:

```
</p><img src=x onerror=alert(1) /><p>
```



**Pic 3**


When the stored content is rendered, the alert is displayed:



**Pic 4**
