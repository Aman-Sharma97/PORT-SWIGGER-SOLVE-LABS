# User ID Controlled by a Request Parameter

The account page in this lab has a horizontal privilege escalation flaw.

**Goal:** Retrieve `carlos`'s API key and submit it as the solution.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/access-control

<img width="768" height="391" alt="1" src="https://github.com/user-attachments/assets/ea2319f6-e9bc-4924-97ba-61f936457ceb" />


---

Clicking "Home" sends this GET request:

<img width="515" height="347" alt="2" src="https://github.com/user-attachments/assets/f4a5f35c-68b2-4950-b882-0ce1c333732b" />


Swapping the `id` value for `carlos` exposes his API key:

<img width="717" height="314" alt="3" src="https://github.com/user-attachments/assets/95235950-0104-43b9-b2ee-ff0faffe57bc" />

