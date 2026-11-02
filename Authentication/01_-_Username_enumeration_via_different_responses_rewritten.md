# Username Enumeration via Different Responses

This lab is susceptible to both username enumeration and password brute-forcing. It has an account whose username and password are both predictable, drawn from the following wordlists:

- Candidate usernames
- Candidate passwords

**Goal:** Enumerate a valid username, brute-force that user's password, then log in and view their account page.

---

**Reference:**
- https://portswigger.net/web-security/authentication/password-based

<img width="770" height="627" alt="1" src="https://github.com/user-attachments/assets/b5a42aa8-fad0-49e5-8554-77f355a194cd" />


---

The login's POST request was sent to Intruder, set to "Pitchfork" attack type (in hindsight this was a mistake — testing every username against every password actually calls for "Cluster bomb", though "Sniper" or "Pitchfork" run faster):

<img width="850" height="542" alt="2" src="https://github.com/user-attachments/assets/8d960bc0-6922-40bd-9d63-850c0f0c0002" />


Usernames were pasted into payload set 1 and passwords into set 2:

<img width="856" height="529" alt="3" src="https://github.com/user-attachments/assets/7625f4e3-72b5-4c6d-8213-98806e0ac41c" />


Looking at response size, one entry stands out — the username "apollo" produces a different-sized response:

<img width="1109" height="709" alt="4" src="https://github.com/user-attachments/assets/456d456e-fa00-45d0-bf91-9ad22d8f5aa4" />


Running a focused attack with the username fixed to "apollo" against all 100 candidate passwords recovers the credentials apollo:batman:

<img width="773" height="457" alt="5" src="https://github.com/user-attachments/assets/80fd4d29-f4c6-45a9-81c9-b9ba0f979ebc" />

