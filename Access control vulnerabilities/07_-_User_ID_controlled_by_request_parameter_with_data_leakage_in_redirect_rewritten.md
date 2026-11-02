# User ID Controlled by a Request Parameter — Data Leaked in a Redirect

This lab's access control flaw leaks sensitive data inside the body of a redirect response.

**Goal:** Retrieve `carlos`'s API key and submit it as the solution.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/access-control

<img width="771" height="169" alt="1" src="https://github.com/user-attachments/assets/64467486-140f-41af-a9f8-ec53cb13fb50" />


---

Clicking "Home" sends this GET request:

<img width="741" height="442" alt="2" src="https://github.com/user-attachments/assets/58b65e63-34fd-4ea5-8627-8b61d10b1eeb" />


Swapping the `id` value for "carlos" triggers a redirect:

<img width="744" height="445" alt="3" src="https://github.com/user-attachments/assets/579b3296-c5c3-43b9-8d3d-3133a05694c2" />


The API key turns out to be present in that redirect response body:

<img width="424" height="336" alt="4" src="https://github.com/user-attachments/assets/45b4dcd8-948d-43b2-86b5-188b554957bc" />

