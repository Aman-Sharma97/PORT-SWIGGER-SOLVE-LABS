
# Reflected XSS with some SVG markup allowed

This lab has a simple reflected XSS vulnerability. The site is blocking common tags but misses some SVG tags and events.

To solve the lab, perform a cross-site scripting attack that calls the alert() function.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/contexts

---------------------------------------------

The search term is reflected inside an `h1` HTML element:



<img width="262" height="62" alt="1" src="https://github.com/user-attachments/assets/95a4e44a-6a95-4c0c-8ab3-b02f90cec38f" />



In this lab, custom tags do not appear to work either, so the tag list is tested systematically:



<img width="777" height="292" alt="2" src="https://github.com/user-attachments/assets/a6383694-8cdf-4daf-8b47-6ee3098d1093" />



The tests show that these tags are accepted:
- animatetransform
- image
- title
- svg



<img width="503" height="103" alt="3" src="https://github.com/user-attachments/assets/b4866c73-a2dd-4558-9c0d-dd58a16be117" />



After identifying accepted tags, the next step is to test their attributes:
- onbegin
 


<img width="503" height="82" alt="4" src="https://github.com/user-attachments/assets/29b8b9f0-64c0-4c04-a5ad-43cf8ce97183" />



The following payload is based on an example from the PortSwigger XSS cheat sheet:



<img width="960" height="110" alt="5" src="https://github.com/user-attachments/assets/09ad18c5-2963-4747-8ff8-345cdc863fdf" />



```
<svg><animatetransform onbegin=alert(1) attributeName=transform>
```



<img width="571" height="173" alt="6" src="https://github.com/user-attachments/assets/ac90eee6-131b-4fb6-b0d4-d5fa57fbb32e" />

