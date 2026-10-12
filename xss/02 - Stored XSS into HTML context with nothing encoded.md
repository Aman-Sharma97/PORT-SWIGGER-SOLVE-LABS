
# Stored XSS into HTML context with nothing encoded

The comment feature stores submitted content and later displays it on the blog post, making this a stored XSS scenario.

To solve this lab, submit a comment that calls the alert function when the blog post is viewed.

---------------------------------------------

Reference: https://portswigger.net/web-security/cross-site-scripting/stored

---------------------------------------------

There is a functionality to post comments in each blog post:



**Pic 1**


Open the blog post again after submitting the comment. The stored input executes when the post is viewed, and the alert appears.



**Pic 2**
