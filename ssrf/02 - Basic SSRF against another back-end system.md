
# Basic SSRF against another back-end system

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, use the stock check functionality to scan the internal 192.168.0.X range for an admin interface on port 8080, then use it to delete the user carlos.

---------------------------------------------

Generated link: https://0aac00f403f0604981937fdd00f60012.web-security-academy.net/

Send a POST request to check the stock:



<img width="811" height="328" alt="1" src="https://github.com/user-attachments/assets/05ef9201-ea35-45f6-9bf4-1a8efdcec3a0" />



Use Burp Intruder to scan the internal address range:



<img width="800" height="285" alt="2" src="https://github.com/user-attachments/assets/607385eb-1c96-4fab-b1bc-3422fd3bd960" />



The host ending in `.169` contains the admin panel:


<img width="514" height="189" alt="3" src="https://github.com/user-attachments/assets/69d1fe0e-ef29-4071-83b5-4e391b07dd26" />




The response indicates that users can be deleted with a GET request:



<img width="561" height="241" alt="4" src="https://github.com/user-attachments/assets/f5ddcccb-d0b1-4e22-9b59-c867a1f39cc0" />



Add the deletion path to the request:



<img width="813" height="327" alt="5" src="https://github.com/user-attachments/assets/85cf97f0-7102-4bba-aaeb-e50d636a1ef4" />



```
POST /product/stock HTTP/2
...
stockApi=http%3A%2F%2F192.168.0.169%3A8080%2Fadmin/delete?username=carlos
```

