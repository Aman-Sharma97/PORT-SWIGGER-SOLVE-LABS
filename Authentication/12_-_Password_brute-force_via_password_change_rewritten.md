# Password Brute-Force via the Password Change Feature

This lab's password-change function can be abused to brute-force an account's password.

**Goal:** Use the provided wordlist of candidate passwords to brute-force Carlos's account and access his "My account" page.

- Your credentials: wiener:peter
- Victim's username: carlos
- Candidate passwords

---

**Reference:**
- https://portswigger.net/web-security/authentication/other-mechanisms

pic 1

---

There's a feature for updating the account's password:

pic 2

It generates a POST request containing the username and current password as parameters:

pic 3

When the current password is correct but the two new password fields don't match, the response reads one way:

pic 4

When the two new passwords also don't match *and* the current password is wrong, the message differs:

pic 5

By changing the username and sending the request to Intruder, every candidate password can be tested in turn:

pic 6

This reveals the correct password: "12345":

pic 7
