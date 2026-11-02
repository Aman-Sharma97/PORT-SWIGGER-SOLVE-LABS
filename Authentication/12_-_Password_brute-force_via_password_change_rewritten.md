# Password Brute-Force via the Password Change Feature

This lab's password-change function can be abused to brute-force an account's password.

**Goal:** Use the provided wordlist of candidate passwords to brute-force Carlos's account and access his "My account" page.

- Your credentials: wiener:peter
- Victim's username: carlos
- Candidate passwords

---

**Reference:**
- https://portswigger.net/web-security/authentication/other-mechanisms

<img width="756" height="210" alt="1" src="https://github.com/user-attachments/assets/7a260596-9180-4ebb-ae89-ad598170288e" />


---

There's a feature for updating the account's password:

<img width="734" height="599" alt="2" src="https://github.com/user-attachments/assets/8dec9b36-4b60-410c-90fe-5e19a6d916ce" />


It generates a POST request containing the username and current password as parameters:
<img width="810" height="317" alt="3" src="https://github.com/user-attachments/assets/38df0e88-db7f-4068-8e21-67edee1cf4e1" />


When the current password is correct but the two new password fields don't match, the response reads one way:

<img width="981" height="525" alt="4" src="https://github.com/user-attachments/assets/034b62a1-2f3f-4239-ad4e-93399dc3e68e" />


When the two new passwords also don't match *and* the current password is wrong, the message differs:

<img width="981" height="526" alt="5" src="https://github.com/user-attachments/assets/0b3e8f67-6d8d-4c01-8c79-b0c22c9c7455" />


By changing the username and sending the request to Intruder, every candidate password can be tested in turn:

<img width="902" height="428" alt="6" src="https://github.com/user-attachments/assets/ba975332-3168-401b-a0bc-c70121c4fa1f" />


This reveals the correct password: "12345":

<img width="686" height="506" alt="7" src="https://github.com/user-attachments/assets/dd863dc5-8bfd-4e2e-9277-0589ead66997" />

