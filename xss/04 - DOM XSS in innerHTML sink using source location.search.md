
# DOM XSS in innerHTML sink using source location.search

The search feature contains DOM-based XSS because a value from the URL query string is assigned to an HTML sink. It uses an innerHTML assignment, which changes the HTML contents of a div element, using data from location.search.

To solve this lab, perform a cross-site scripting attack that calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


The search page accepts its query through the `/?search=` parameter:



**Pic 1**


The relevant source code shows the sink where the URL-controlled value is written:

```
function doSearchQuery(query) {
    document.getElementById('searchMessage').innerHTML = query;
}
var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
    doSearchQuery(query);
}
```



**Pic 2**


The `searchMessage` span is populated using the request's `search` GET parameter. Because that value is inserted as HTML, the following payload triggers the alert:

```
<img src=x onerror=alert(1) />
```

**Pic 3**
