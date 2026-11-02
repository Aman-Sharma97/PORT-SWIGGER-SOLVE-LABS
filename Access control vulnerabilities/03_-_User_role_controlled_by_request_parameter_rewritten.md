# User Role Controlled by a Request Parameter

This lab's admin panel at `/admin` identifies administrators using a cookie that can be freely forged.

**Goal:** Reach the admin panel and use it to delete the user `carlos`.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/access-control

<img width="768" height="342" alt="1" src="https://github.com/user-attachments/assets/f5398d1a-6bae-41ed-9aee-dda39a461c5a" />


---

After logging in, a redirect sets the `Admin` cookie's value to `false`:

<img width="699" height="234" alt="2" src="https://github.com/user-attachments/assets/91e9183b-0441-4d8f-9cb9-dfaa65204adb" />


Flipping that value to `true` grants visibility into the admin panel, where the user can then be deleted:
<img width="794" height="329" alt="3" src="https://github.com/user-attachments/assets/b2a87175-5f22-4a77-b6e4-3e8e7cdf709d" />

<img width="786" height="174" alt="4" src="https://github.com/user-attachments/assets/7a5c7ad1-fd3d-494e-bc0f-0b5bdb5888cc" />

