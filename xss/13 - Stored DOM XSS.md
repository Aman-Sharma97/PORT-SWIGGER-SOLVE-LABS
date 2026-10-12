
# Stored DOM XSS

This lab demonstrates a stored DOM vulnerability in the blog comment functionality. To solve this lab, exploit this vulnerability to call the alert() function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------

The blog post provides a comment form, allowing content to be submitted:


<img width="717" height="595" alt="1" src="https://github.com/user-attachments/assets/70ae6ae7-a246-4f22-87fa-0c96bd7c7b7b" />



After submission, the comment is represented by the following HTML:



<img width="333" height="188" alt="2" src="https://github.com/user-attachments/assets/99741c9c-c698-4621-a7c1-e0eff2385079" />


We can try the payload:

```
</p><img src=x onerror=alert(1) /><p>
```



<img width="693" height="598" alt="3" src="https://github.com/user-attachments/assets/0170c8df-502e-4643-9518-5659e57d5c5b" />



When the stored content is rendered, the alert is displayed:



<img width="666" height="167" alt="4" src="https://github.com/user-attachments/assets/5c9c94e1-aaa2-4a1e-9d21-25983fd93d0a" />

