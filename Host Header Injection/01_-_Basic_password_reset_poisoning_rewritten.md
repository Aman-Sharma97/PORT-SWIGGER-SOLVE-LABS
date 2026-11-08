# Basic Password Reset Poisoning

This lab is susceptible to password reset poisoning. The user carlos is modeled to click on any link that arrives in his inbox without question.

**Goal:** Log into Carlos's account.

Login credentials for your own account: wiener:peter. Any email sent to this account can be read through the email client on the exploit server.

---

**Reference:**
- https://portswigger.net/web-security/host-header/exploiting/password-reset-poisoning

<img width="773" height="637" alt="1" src="https://github.com/user-attachments/assets/765f2473-b5ee-4382-878e-2c107762dfda" />


---

There's a feature for recovering forgotten passwords:

<img width="458" height="208" alt="2" src="https://github.com/user-attachments/assets/60ccc967-3624-4ac7-9574-d74b4eff4dfe" />


It generates a POST request for the user "wiener":

<img width="557" height="454" alt="3" src="https://github.com/user-attachments/assets/434d8ef2-c09f-43e3-b0cd-24c2685ed37f" />


The `Host` header on that POST request to `/forgot-password` can be changed, along with the `username` parameter being set to "carlos":

<img width="992" height="492" alt="4" src="https://github.com/user-attachments/assets/257d93eb-0b50-45f2-80d7-9ff7cce58450" />


A request then arrives at the exploit server:

<img width="928" height="74" alt="5" src="https://github.com/user-attachments/assets/6b12b35b-7178-43f1-b778-789e2c46582d" />


The token from that request can be used to set a new password:

<img width="432" height="277" alt="6" src="https://github.com/user-attachments/assets/df5f3559-b4e5-4906-acfe-2b1a6e8e1dbf" />


And log into the account as carlos:

<img width="717" height="293" alt="7" src="https://github.com/user-attachments/assets/c900105f-3aa6-46b4-b762-3ad7b583835f" />

