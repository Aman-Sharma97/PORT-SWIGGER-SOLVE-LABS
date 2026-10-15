
# Basic SSRF against another back-end system

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, use the stock check functionality to scan the internal 192.168.0.X range for an admin interface on port 8080, then use it to delete the user carlos.

---------------------------------------------

Generated link: https://0aac00f403f0604981937fdd00f60012.web-security-academy.net/

Send a POST request to check the stock:



**Pic 1**


Use Burp Intruder to scan the internal address range:



**Pic 2**


The host ending in `.169` contains the admin panel:



**Pic 3**


The response indicates that users can be deleted with a GET request:



**Pic 4**


Add the deletion path to the request:



**Pic 5**


```
POST /product/stock HTTP/2
...
stockApi=http%3A%2F%2F192.168.0.169%3A8080%2Fadmin/delete?username=carlos
```

