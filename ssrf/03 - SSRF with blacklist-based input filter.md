
# SSRF with blacklist-based input filter

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos.

The developer has deployed two weak anti-SSRF defenses that you will need to bypass.


---------------------------------------------

References: 

- https://portswigger.net/web-security/ssrf



<img width="846" height="297" alt="1" src="https://github.com/user-attachments/assets/c77e3c3d-5a34-44a9-88b7-fb36c578ad7a" />


---------------------------------------------


A button is available to check the stock:



<img width="470" height="87" alt="2" src="https://github.com/user-attachments/assets/39a5de33-e202-447c-85df-e29b0f4b6e08" />



Clicking it generates the following POST request:



<img width="980" height="462" alt="3" src="https://github.com/user-attachments/assets/c1622997-12d5-4524-bcf6-a93813390b6f" />



The `stockApi` parameter contains the URL “http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1”. We can try changing it to a different URL:



<img width="696" height="313" alt="4" src="https://github.com/user-attachments/assets/522e3e39-7147-46f1-beec-4c95c2e7a1d5" />



The filter appears to block “http://127.0.0.1” and “http://localhost/”:


<img width="979" height="432" alt="5" src="https://github.com/user-attachments/assets/a52c6a36-978c-4e87-a75a-817b93dcdf04" />



Using `http://017700000001/` changes the error response:


<img width="987" height="428" alt="6" src="https://github.com/user-attachments/assets/4a517274-368d-484e-8fc5-5102fb631aa9" />



Using `http://127.1`, we can reach the local server—for example, the `/login` page. However, access to `/admin` is still detected and blocked.



<img width="985" height="546" alt="7" src="https://github.com/user-attachments/assets/d2dcf218-780d-44d9-b372-ccfc950d2a08" />



Changing the path to uppercase `ADMIN` allows access to `/admin`:



<img width="986" height="436" alt="8" src="https://github.com/user-attachments/assets/6cc7a1a1-30e6-4000-bfdc-1fefaaa1d387" />



The source code shows that the user can be deleted with a GET request using a payload like this:

```
POST /product/stock HTTP/2
...

stockApi=http://127.1/ADMIN/delete?username=carlos
```



<img width="977" height="432" alt="9" src="https://github.com/user-attachments/assets/f220c523-a9a6-4591-8cfc-834d6bf70e3b" />
