
# Reflected XSS into attribute with angle brackets HTML-encoded

This lab contains a reflected cross-site scripting vulnerability in the search blog functionality where angle brackets are HTML-encoded. To solve this lab, perform a cross-site scripting attack that injects an attribute and calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/contexts



<img width="845" height="393" alt="1" src="https://github.com/user-attachments/assets/07a2a8a4-05fe-4939-a798-9f8b87194299" />


---------------------------------------------

Searching for `aaaa` shows that the supplied text is placed in this HTML element's `value` attribute:



<img width="602" height="91" alt="2" src="https://github.com/user-attachments/assets/ae8b1ecb-bae5-41ea-9934-0ab86dc9812a" />


This payload causes the alert to appear:

```
" autofocus onfocus=alert(1) x="
```



<img width="716" height="169" alt="3" src="https://github.com/user-attachments/assets/f90f5d5a-2df5-4131-af64-eb9021478e80" />



This is the HTML content which explains why the payload worked:



<img width="798" height="93" alt="4" src="https://github.com/user-attachments/assets/659498e2-d92d-4e85-8d47-e57e1e7ab334" />


