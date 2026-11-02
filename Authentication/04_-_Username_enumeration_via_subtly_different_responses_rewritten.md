# Username Enumeration via Subtly Different Responses

This lab is vulnerable to username enumeration and password brute-forcing in a more subtle way. It has an account whose username and password are both predictable, drawn from the following wordlists:

- Candidate usernames
- Candidate passwords

**Goal:** Enumerate a valid username, brute-force that user's password, then log in and view their account page.

---

**Reference:**
- https://portswigger.net/web-security/authentication/password-based

<img width="774" height="820" alt="1" src="https://github.com/user-attachments/assets/4f8a4b07-0fbe-474a-b935-f97412557556" />


---

First, a Sniper attack in Intruder was used to enumerate usernames:

<img width="782" height="356" alt="2" src="https://github.com/user-attachments/assets/54d82e70-c8d0-4f92-a1f1-8f3c1cf2977b" />


For some usernames, the response includes an HTML comment and ends up larger than 3100 bytes in size, while other usernames' responses are missing that comment on line 14:

<img width="691" height="417" alt="3" src="https://github.com/user-attachments/assets/9adfbe9b-67a5-443d-b32b-2c91f74d4511" />

<img width="698" height="383" alt="4" src="https://github.com/user-attachments/assets/0fe0a444-38c8-4675-8c5a-be7466065dea" />

<img width="1079" height="447" alt="5" src="https://github.com/user-attachments/assets/2f9cc675-48d7-432a-9e48-8371c68e1549" />


Adding an extra column reveals that one response is also missing the trailing "." character:

<img width="777" height="481" alt="6" src="https://github.com/user-attachments/assets/bcda157b-2b69-4866-b2f9-d32b1164d1b6" />


Testing further, a redirect appears when the correct password is used — giving the credentials akamai:monitoring:

<img width="658" height="112" alt="7" src="https://github.com/user-attachments/assets/8f806d7a-0c18-4738-9d74-7c0a18303fc4" />

