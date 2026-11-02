# Username Enumeration via Response Timing

This lab leaks valid usernames through how long its responses take. 

**Goal:** Enumerate a valid username, brute-force that user's password, then log in and view their account page.

Your credentials: wiener:peter

- Candidate usernames
- Candidate passwords

Hint: The lab also applies a form of IP-based brute-force protection, but this can be sidestepped just by manipulating HTTP headers.

---

**Reference:**
- https://portswigger.net/web-security/authentication/password-based

<img width="774" height="820" alt="1" src="https://github.com/user-attachments/assets/9e4ecdb7-737c-464c-8748-b2953844085e" />


---

A Pitchfork attack was configured, adding an `X-Forwarded-For` header as an extra injection point:

<img width="816" height="498" alt="2" src="https://github.com/user-attachments/assets/43c0a014-e30d-44af-9db8-059794d3f922" />


Incrementing numbers were appended to the spoofed IP in that header:

<img width="590" height="398" alt="3" src="https://github.com/user-attachments/assets/ee4b633f-7804-425d-9b70-b327cf29606f" />


The "Columns" button at the top lets extra data be shown:

<img width="872" height="563" alt="4" src="https://github.com/user-attachments/assets/6750cd3a-535f-416e-b2bc-5f7d9e094428" />


This makes it possible to see which requests took noticeably longer — using a very long password string here is key to making the timing difference clear:

<img width="826" height="120" alt="5" src="https://github.com/user-attachments/assets/cb4989eb-f49d-4ce0-a546-e61585a0dcbb" />


A redirect eventually appears, giving the credentials accounts:jessica:

<img width="685" height="129" alt="6" src="https://github.com/user-attachments/assets/64817c22-9f2f-4149-9136-55079de9a8cd" />

