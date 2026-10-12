
# DOM XSS in jQuery anchor href attribute sink using location.search source

The Submit Feedback page contains DOM-based XSS: a URL parameter is used to set an anchor element's `href` through jQuery. It uses the jQuery library's $ selector function to find an anchor element, and changes its href attribute using data from location.search.

To solve this lab, make the "back" link alert document.cookie.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


The vulnerable sink appears on the Submit Feedback page:



<img width="732" height="77" alt="1" src="https://github.com/user-attachments/assets/9b4b1fd7-702d-4090-a48e-40b190a820f1" />



The Submit Feedback page uses this URL, including the `returnPath` parameter:



<img width="156" height="29" alt="2" src="https://github.com/user-attachments/assets/57971b8f-18df-4f94-a7bc-313fc84fd23e" />


A JavaScript URL can be supplied through that parameter, for example:

```
/feedback?returnPath=javascript:alert(document.cookie)
```




<img width="704" height="209" alt="3" src="https://github.com/user-attachments/assets/b2001495-8a56-4d68-91df-fff7d3dc169d" />

