
# Reflected XSS with some SVG markup allowed

This lab has a simple reflected XSS vulnerability. The site is blocking common tags but misses some SVG tags and events.

To solve the lab, perform a cross-site scripting attack that calls the alert() function.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/contexts

---------------------------------------------

The search term is reflected inside an `h1` HTML element:



**Pic 1**


In this lab, custom tags do not appear to work either, so the tag list is tested systematically:



**Pic 2**


The tests show that these tags are accepted:
- animatetransform
- image
- title
- svg



**Pic 3**


After identifying accepted tags, the next step is to test their attributes:
- onbegin
 


**Pic 4**


The following payload is based on an example from the PortSwigger XSS cheat sheet:



**Pic 5**


```
<svg><animatetransform onbegin=alert(1) attributeName=transform>
```



**Pic 6**
