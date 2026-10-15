
# Basic SSRF against the local server

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos.

---------------------------------------------

References: 

- https://portswigger.net/web-security/ssrf



<img width="846" height="821" alt="1" src="https://github.com/user-attachments/assets/d4739b58-1b2e-46c8-9b3e-8b9311d0492a" />


---------------------------------------------

A button is available to check the stock:



<img width="470" height="87" alt="2" src="https://github.com/user-attachments/assets/4c3df262-82d8-4c41-afb1-5201a4e1393b" />



Clicking it generates the following POST request:



<img width="890" height="292" alt="3" src="https://github.com/user-attachments/assets/cb0ee75e-7541-477c-be07-1d3686e4a7f7" />



The `stockApi` parameter contains the URL “http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1”. We can try changing it to a different URL:



<img width="696" height="313" alt="4" src="https://github.com/user-attachments/assets/8c8f0c34-a32b-4445-ad7d-62a8b01c9334" />



For example, we can try to retrieve the contents of the `/admin` page:

```
POST /product/stock HTTP/2
...

stockApi=http://localhost/admin
```



<img width="951" height="449" alt="5" src="https://github.com/user-attachments/assets/e4ef59d6-2d77-4ca4-ba2f-460ebf978050" />


The source code shows that a user can be deleted by sending a GET request:



<img width="358" height="238" alt="6" src="https://github.com/user-attachments/assets/886878db-f906-4f6f-99e3-55b911c2fe3d" />



We can therefore delete the user “carlos” using this payload:

```
POST /product/stock HTTP/2
...

stockApi=http://localhost/admin/delete?username=carlos
```



<img width="945" height="462" alt="7" src="https://github.com/user-attachments/assets/fa3ea65a-6605-4d5f-a362-b5c14239ad3a" />
