
# Stored XSS into anchor href attribute with double quotes HTML-encoded

The comment feature stores submitted content and later displays it on the blog post, making this a stored XSS scenario. To solve this lab, submit a comment that calls the alert function when the comment author name is clicked.


---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/contexts



<img width="855" height="139" alt="1" src="https://github.com/user-attachments/assets/b6bdaf02-04c6-4353-a790-0ea41cea53e1" />


---------------------------------------------

The blog post provides a comment form, allowing content to be submitted:


<img width="725" height="604" alt="2" src="https://github.com/user-attachments/assets/048c969e-9972-4a83-89b8-6b9c2a77b078" />


This is the HTML element generated:



<img width="343" height="209" alt="3" src="https://github.com/user-attachments/assets/45359fbe-cd32-478a-9a55-df49fecd3e7c" />



When the user name is clicked, it redirects to the website set in the comment:





<img width="204" height="73" alt="4" src="https://github.com/user-attachments/assets/4337d91d-7c46-41b2-a39f-20ead95fa10a" />

<img width="769" height="197" alt="5" src="https://github.com/user-attachments/assets/1035f372-126a-4396-9dea-b4aa5afecb16" />


We will set the website to a javascript url:

```
javascript:alert(1)
```



<img width="679" height="592" alt="6" src="https://github.com/user-attachments/assets/36a68332-a44d-466c-a196-f774b184366d" />



When clicked, the alert pops:


<img width="706" height="223" alt="7" src="https://github.com/user-attachments/assets/11ccbcd3-3426-43eb-96e6-7f475037e7ef" />

