# Bypassing Method-Based Access Control

This lab enforces access control partly based on the HTTP method of the request. The admin panel can be explored first by logging in with administrator:admin.

**Goal:** Log in as wiener:peter and exploit the flawed access control to promote your own account to administrator.

---

**Reference:**
- https://portswigger.net/web-security/access-control

<img width="762" height="189" alt="1" src="https://github.com/user-attachments/assets/b20da443-319f-4f5a-aa18-d4bd00a8472f" />


---

The admin panel includes a feature for upgrading users:

<img width="715" height="147" alt="2" src="https://github.com/user-attachments/assets/a04a4f01-b455-44a8-9be9-ae3aa6cd2f36" />


This action is normally a POST request:

<img width="742" height="484" alt="3" src="https://github.com/user-attachments/assets/a67c39a3-249c-4495-b0d9-210234e94007" />


But it turns out to work just as well as a GET request:

<img width="741" height="421" alt="4" src="https://github.com/user-attachments/assets/f42e3c9f-5a0c-4d2b-a58c-7a22b42c739e" />


Running this as wiener successfully promotes the account to administrator:

<img width="741" height="368" alt="5" src="https://github.com/user-attachments/assets/3da2b17c-66c5-467e-90a6-5aabd0db89a8" />

