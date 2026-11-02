# Username Enumeration via Subtly Different Responses

This lab is vulnerable to username enumeration and password brute-forcing in a more subtle way. It has an account whose username and password are both predictable, drawn from the following wordlists:

- Candidate usernames
- Candidate passwords

**Goal:** Enumerate a valid username, brute-force that user's password, then log in and view their account page.

---

**Reference:**
- https://portswigger.net/web-security/authentication/password-based

pic 1

---

First, a Sniper attack in Intruder was used to enumerate usernames:

pic 2

For some usernames, the response includes an HTML comment and ends up larger than 3100 bytes in size, while other usernames' responses are missing that comment on line 14:

pic 3
pic 4
pic 5

Adding an extra column reveals that one response is also missing the trailing "." character:

pic 6

Testing further, a redirect appears when the correct password is used — giving the credentials akamai:monitoring:

pic 7
