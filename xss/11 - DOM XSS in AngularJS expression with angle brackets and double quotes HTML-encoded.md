
# DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

This lab contains a DOM-based cross-site scripting vulnerability in a AngularJS expression within the search functionality.

AngularJS is a popular JavaScript library, which scans the contents of HTML nodes containing the ng-app attribute (also known as an AngularJS directive). When a directive is added to the HTML code, you can execute JavaScript expressions within double curly braces. This technique is useful when angle brackets are being encoded.

To solve this lab, perform a cross-site scripting attack that executes an AngularJS expression and calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------

There is a search function:



**Pic 1**


The reflected search string appears inside an `h1` element:



**Pic 2**


Testing the input `<'>` shows that `>`, `<`, and `'` are HTML-encoded:



**Pic 3**


The application evaluates AngularJS expressions written with curly braces. This test expression is interpreted:

```
{{1== 1 ? "Yes, it is equal" : "No, it is not"}}
```





**Pic 4**
**Pic 5**


An alert can also be triggered with the following expression (example reference: https://stackoverflow.com/questions/66759842/what-does-object-constructor-constructoralert1-actually-do-in-javascript):

```
 {{constructor.constructor('alert(1)')()}}
```



**Pic 6**


The official solution is similar: {{$on.constructor('alert(1)')()}}
