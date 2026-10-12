
# Reflected XSS into HTML context with nothing encoded

The search feature in this lab reflects user-supplied input into the page without encoding it, creating a reflected XSS issue.

To solve the lab, perform a cross-site scripting attack that calls the alert function.

---------------------------------------------

Reference: https://portswigger.net/web-security/cross-site-scripting/reflected

---------------------------------------------

The search feature places the supplied value into the generated HTML, as shown in the screenshots below:





<img width="673" height="211" alt="1" src="https://github.com/user-attachments/assets/4ba89283-e8a6-4cce-9670-3a2e9f14962e" />

<img width="685" height="168" alt="2" src="https://github.com/user-attachments/assets/c8b0da9a-8b22-4a3c-b452-6a23af0c745c" />



Enter `<script>alert(1)</script>` in the search field; the browser then displays the alert, confirming the reflected input executes.




<img width="677" height="209" alt="3" src="https://github.com/user-attachments/assets/bab2caca-2c6c-44f5-afc9-6b7e195f2b26" />


<img width="623" height="171" alt="4" src="https://github.com/user-attachments/assets/b7c767fb-b68a-4535-b1c2-f3b5e103c36f" />

