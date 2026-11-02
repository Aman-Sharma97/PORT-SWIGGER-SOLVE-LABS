# Two-Factor Authentication — Broken Logic

This lab's 2FA implementation has a flawed logic flow that can be exploited.

**Goal:** Reach Carlos's account page.

Your credentials: wiener:peter
Victim's username: carlos
Access to the email server is also available, to receive your own 2FA code.

Hint: Carlos never logs into the site himself.

---

**Reference:**
- https://portswigger.net/web-security/authentication/multi-factor

<img width="769" height="915" alt="1" src="https://github.com/user-attachments/assets/bf595c9e-7895-456b-aaa3-e15f1ccb118a" />


---

A security code sent via email is required to complete login:

<img width="718" height="186" alt="2" src="https://github.com/user-attachments/assets/a7f6af5e-808b-4127-af34-a65b2d529f5b" />


After the first login step, a redirect sets a cookie with the value `verify=wiener`:

<img width="709" height="154" alt="3" src="https://github.com/user-attachments/assets/3dd70284-acde-4a13-a28b-352fd2fb3921" />


Changing that value to `verify=carlos` is possible:

<img width="724" height="150" alt="4" src="https://github.com/user-attachments/assets/4009a90a-0be4-4554-ad2d-99b8b056ec4f" />


With that cookie set, reaching `/login2` now shows the verification step for carlos — but no MFA code is actually sent to us, since it's his email that would receive it:

<img width="775" height="269" alt="5" src="https://github.com/user-attachments/assets/b5c93fbe-700b-4645-811a-e423a525f401" />


This request was sent to Intruder:

<img width="777" height="303" alt="6" src="https://github.com/user-attachments/assets/b6304ab3-e222-46d5-91af-88560ca96aaf" />


A brute-force attack was set up using a payload like this:

<img width="602" height="526" alt="7" src="https://github.com/user-attachments/assets/7a44557b-6079-4351-8621-de6959814dce" />


The code "1985" triggers a different response from the rest:

<img width="714" height="264" alt="8" src="https://github.com/user-attachments/assets/5642d1ab-6f29-4b96-8c68-91daaf1d9e72" />


Submitting it successfully logs in as carlos:

<img width="717" height="307" alt="9" src="https://github.com/user-attachments/assets/71cfe938-5363-4f7d-8980-1696c5c7ffda" />

