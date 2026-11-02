# User ID Controlled by a Request Parameter — With Password Disclosure

The account page in this lab prefills the logged-in user's existing password into a masked input field.

**Goal:** Retrieve the administrator's password and use it to delete `carlos`.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/access-control

<img width="771" height="417" alt="1" src="https://github.com/user-attachments/assets/9827c639-55ad-4402-906b-caf200f31c7b" />


---

Clicking "Home" sends this GET request:

<img width="743" height="446" alt="2" src="https://github.com/user-attachments/assets/25efcb26-2f3f-4724-a816-f0be3dd58b25" />


The `id` value can be swapped for "administrator":

<img width="740" height="725" alt="3" src="https://github.com/user-attachments/assets/102dd4cb-c18f-4eae-9c3f-0f2b90afa822" />


Switching the password field's input type to "text" reveals it in plain view:

<img width="719" height="360" alt="4" src="https://github.com/user-attachments/assets/b55171f1-019d-49a4-bc01-6f9559068ffc" />


From there, the target user can be deleted:

<img width="679" height="299" alt="5" src="https://github.com/user-attachments/assets/4fd55bd3-b3b6-45a1-9819-a02168d946df" />

