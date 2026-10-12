
# DOM XSS in jQuery anchor href attribute sink using location.search source

The Submit Feedback page contains DOM-based XSS: a URL parameter is used to set an anchor element's `href` through jQuery. It uses the jQuery library's $ selector function to find an anchor element, and changes its href attribute using data from location.search.

To solve this lab, make the "back" link alert document.cookie.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


The vulnerable sink appears on the Submit Feedback page:



**Pic 1**


The Submit Feedback page uses this URL, including the `returnPath` parameter:



**Pic 2**

A JavaScript URL can be supplied through that parameter, for example:

```
/feedback?returnPath=javascript:alert(document.cookie)
```




**Pic 3**
