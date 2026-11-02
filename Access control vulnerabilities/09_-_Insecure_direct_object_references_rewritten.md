# Insecure Direct Object References

This lab saves users' chat logs as plain files on the server and serves them through predictable static URLs.

**Goal:** Find `carlos`'s password and log into his account with it.

---

**Reference:**
- https://portswigger.net/web-security/access-control

<img width="762" height="228" alt="1" src="https://github.com/user-attachments/assets/e9863bf5-0d4a-4334-9acb-cbc18d3141b2" />


---

There's a live chat feature:

<img width="719" height="344" alt="2" src="https://github.com/user-attachments/assets/b7456bef-768d-4506-8321-abb1a490be67" />


Its transcript can be downloaded:

<img width="1043" height="341" alt="3" src="https://github.com/user-attachments/assets/75faa8df-e938-4875-b3b4-f50eabd38792" />


Swapping the filename for `1.txt` reveals the password:

<img width="1050" height="383" alt="4" src="https://github.com/user-attachments/assets/c5c19408-b6c7-46da-a8a9-07b968d5af60" />

