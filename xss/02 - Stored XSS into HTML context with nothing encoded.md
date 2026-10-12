
# Stored XSS into HTML context with nothing encoded

The comment feature stores submitted content and later displays it on the blog post, making this a stored XSS scenario.

To solve this lab, submit a comment that calls the alert function when the blog post is viewed.

---------------------------------------------

Reference: https://portswigger.net/web-security/cross-site-scripting/stored

---------------------------------------------

There is a functionality to post comments in each blog post:



<img width="677" height="593" alt="1" src="https://github.com/user-attachments/assets/e36669b9-7793-4aaf-ab32-2b9540f98996" />



Open the blog post again after submitting the comment. The stored input executes when the post is viewed, and the alert appears.



<img width="613" height="166" alt="2" src="https://github.com/user-attachments/assets/4ad89b35-20db-437f-b0aa-41fbcd5d3edd" />

