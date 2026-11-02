# User ID Controlled by a Request Parameter — With Unpredictable User IDs

The account page in this lab has a horizontal privilege escalation flaw, but users are identified by GUIDs instead of simple usernames.

**Goal:** Find `carlos`'s GUID, then retrieve and submit his API key.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/access-control

<img width="765" height="202" alt="1" src="https://github.com/user-attachments/assets/5a3275dc-bf5c-4193-96a6-52dbd5af8fa6" />


---

Clicking "Home" sends this GET request:

<img width="739" height="475" alt="2" src="https://github.com/user-attachments/assets/dcef922e-f117-4356-9330-52b360450fd4" />


The administrator's GUID can be found by inspecting the blog posts:

<img width="932" height="122" alt="3" src="https://github.com/user-attachments/assets/f71e734e-57de-405f-b2b8-79baada0feab" />


That GUID is then used to craft a new request:

<img width="742" height="474" alt="4" src="https://github.com/user-attachments/assets/bf13526d-7584-429b-93f8-a972914125ce" />

