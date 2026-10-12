
# Reflected XSS into HTML context with nothing encoded

The search feature in this lab reflects user-supplied input into the page without encoding it, creating a reflected XSS issue.

To solve the lab, perform a cross-site scripting attack that calls the alert function.

---------------------------------------------

Reference: https://portswigger.net/web-security/cross-site-scripting/reflected

---------------------------------------------

The search feature places the supplied value into the generated HTML, as shown in the screenshots below:





**Pic 1**
**Pic 2**


Enter `<script>alert(1)</script>` in the search field; the browser then displays the alert, confirming the reflected input executes.





**Pic 3**
**Pic 4**
