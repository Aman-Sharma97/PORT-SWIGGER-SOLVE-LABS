
# SSRF with blacklist-based input filter

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos.

The developer has deployed two weak anti-SSRF defenses that you will need to bypass.


---------------------------------------------

References: 

- https://portswigger.net/web-security/ssrf



**Pic 1**

---------------------------------------------


A button is available to check the stock:



**Pic 2**


Clicking it generates the following POST request:



**Pic 3**


The `stockApi` parameter contains the URL “http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1”. We can try changing it to a different URL:



**Pic 4**


The filter appears to block “http://127.0.0.1” and “http://localhost/”:



**Pic 5**


Using `http://017700000001/` changes the error response:



**Pic 6**


Using `http://127.1`, we can reach the local server—for example, the `/login` page. However, access to `/admin` is still detected and blocked.



**Pic 7**


Changing the path to uppercase `ADMIN` allows access to `/admin`:



**Pic 8**


The source code shows that the user can be deleted with a GET request using a payload like this:

```
POST /product/stock HTTP/2
...

stockApi=http://127.1/ADMIN/delete?username=carlos
```



**Pic 9**