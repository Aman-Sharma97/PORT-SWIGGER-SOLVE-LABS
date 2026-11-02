# Username Enumeration via Differing Responses

This lab is susceptible to both username enumeration and password brute-forcing. It has an account whose username and password are both predictable, drawn from the following wordlists:

- Candidate usernames
- Candidate passwords

**Goal:** Enumerate a valid username, brute-force that user's password, then log in and view their account page.

---

**Reference:**
- https://portswigger.net/web-security/authentication/password-based

pic 1

---

The login's POST request was sent to Intruder, set to "Pitchfork" attack type (in hindsight this was a mistake — testing every username against every password actually calls for "Cluster bomb", though "Sniper" or "Pitchfork" run faster):

pic 2

Usernames were pasted into payload set 1 and passwords into set 2:

pic 3

Looking at response size, one entry stands out — the username "apollo" produces a different-sized response:

pic 4

Running a focused attack with the username fixed to "apollo" against all 100 candidate passwords recovers the credentials apollo:batman:

pic 5
