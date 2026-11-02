# Username Enumeration via Account Lockout

This lab is vulnerable to username enumeration. It locks accounts after repeated failures, but that mechanism has a logic flaw.

**Goal:** Enumerate a valid username, brute-force that user's password, then log in and view their account page.

- Candidate usernames
- Candidate passwords

---

**Reference:**
- https://portswigger.net/web-security/authentication/password-based

<img width="767" height="202" alt="1" src="https://github.com/user-attachments/assets/114aa23b-7dbb-4668-b3fa-6c3128305911" />


---

The login's POST request was intercepted and sent to Intruder:

<img width="785" height="302" alt="2" src="https://github.com/user-attachments/assets/050bb29e-a034-45f1-b7bc-e99e99b73364" />


The user "pi" ends up locked:

<img width="1095" height="741" alt="3" src="https://github.com/user-attachments/assets/37cf6b9b-51f6-4a6d-aa38-346b69d64259" />


With that username fixed, every candidate password is then tested:

<img width="780" height="302" alt="4" src="https://github.com/user-attachments/assets/cfd3b569-d555-468a-bfa9-17afc3bd1391" />


One request stands out — rather than an "invalid password" or "too many attempts" message, it returns a blank response, which turns out to mark the correct password:

<img width="1095" height="779" alt="5" src="https://github.com/user-attachments/assets/b360f4f3-2795-4c05-817a-51aadc156135" />

