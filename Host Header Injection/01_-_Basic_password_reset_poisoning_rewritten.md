# Basic Password Reset Poisoning

This lab is susceptible to password reset poisoning. The user carlos is modeled to click on any link that arrives in his inbox without question.

**Goal:** Log into Carlos's account.

Login credentials for your own account: wiener:peter. Any email sent to this account can be read through the email client on the exploit server.

---

**Reference:**
- https://portswigger.net/web-security/host-header/exploiting/password-reset-poisoning

pic 1

---

There's a feature for recovering forgotten passwords:

pic 2

It generates a POST request for the user "wiener":

pic 3

The `Host` header on that POST request to `/forgot-password` can be changed, along with the `username` parameter being set to "carlos":

pic 4

A request then arrives at the exploit server:

pic 5

The token from that request can be used to set a new password:

pic 6

And log into the account as carlos:

pic 7
