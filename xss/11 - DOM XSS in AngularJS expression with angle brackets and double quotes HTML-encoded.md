
# DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

This lab contains a DOM-based cross-site scripting vulnerability in a AngularJS expression within the search functionality.

AngularJS is a popular JavaScript library, which scans the contents of HTML nodes containing the ng-app attribute (also known as an AngularJS directive). When a directive is added to the HTML code, you can execute JavaScript expressions within double curly braces. This technique is useful when angle brackets are being encoded.

To solve this lab, perform a cross-site scripting attack that executes an AngularJS expression and calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------

There is a search function:


<img width="725" height="217" alt="1" src="https://github.com/user-attachments/assets/f86accb4-9bbe-4333-b87b-4f415d330e02" />



The reflected search string appears inside an `h1` element:



<img width="341" height="61" alt="2" src="https://github.com/user-attachments/assets/fa075d67-eb0d-4cbd-a837-84a0e367e412" />



Testing the input `<'>` shows that `>`, `<`, and `'` are HTML-encoded:



<img width="365" height="63" alt="3" src="https://github.com/user-attachments/assets/8b5621da-e8c8-4829-83de-ab0c61aef62e" />



The application evaluates AngularJS expressions written with curly braces. This test expression is interpreted:

```
{{1== 1 ? "Yes, it is equal" : "No, it is not"}}
```





<img width="719" height="163" alt="4" src="https://github.com/user-attachments/assets/76cdd3b1-ed38-402c-bcfb-16af85ab926d" />

<img width="604" height="62" alt="5" src="https://github.com/user-attachments/assets/27103d64-560f-409e-a2e1-4b99d2cff67f" />



An alert can also be triggered with the following expression (example reference: https://stackoverflow.com/questions/66759842/what-does-object-constructor-constructoralert1-actually-do-in-javascript):

```
 {{constructor.constructor('alert(1)')()}}
```


<img width="855" height="169" alt="6" src="https://github.com/user-attachments/assets/47176029-77ad-4818-8041-d4a4194b7e40" />



The official solution is similar: {{$on.constructor('alert(1)')()}}
