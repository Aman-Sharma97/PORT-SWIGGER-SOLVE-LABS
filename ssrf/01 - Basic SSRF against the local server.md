
# Basic SSRF against the local server

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos.

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


For example, we can try to retrieve the contents of the `/admin` page:

```
POST /product/stock HTTP/2
...

stockApi=http://localhost/admin
```



**Pic 5**

The source code shows that a user can be deleted by sending a GET request:



**Pic 6**


We can therefore delete the user “carlos” using this payload:

```
POST /product/stock HTTP/2
...

stockApi=http://localhost/admin/delete?username=carlos
```



**Pic 7**