
# DOM XSS in innerHTML sink using source location.search

The search feature contains DOM-based XSS because a value from the URL query string is assigned to an HTML sink. It uses an innerHTML assignment, which changes the HTML contents of a div element, using data from location.search.

To solve this lab, perform a cross-site scripting attack that calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


The search page accepts its query through the `/?search=` parameter:


<img width="719" height="215" alt="1" src="https://github.com/user-attachments/assets/c0d2f035-93d7-41aa-b20a-36b6b9f2a5df" />



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



<img width="648" height="152" alt="2" src="https://github.com/user-attachments/assets/e56fb10b-6d5a-4d3a-940a-4e31bb9aa6e7" />



The `searchMessage` span is populated using the request's `search` GET parameter. Because that value is inserted as HTML, the following payload triggers the alert:

```
<img src=x onerror=alert(1) />
```

<img width="713" height="169" alt="3" src="https://github.com/user-attachments/assets/c95d63f6-2295-4dc4-b49b-cf446b7225a3" />

