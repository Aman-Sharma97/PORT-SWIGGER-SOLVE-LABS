# Getting Around SameSite=Lax Using a Method Override

The change-email function here is vulnerable to CSRF. The goal is to craft a CSRF attack, hosted on the exploit server, that changes the victim's email.

Login credentials for your own account: wiener:peter

Note: SameSite cookie defaults vary by browser. Since the victim is simulated as using Chrome, it's best to test the exploit in Chrome too (or Burp's bundled Chromium browser).

Hint: Registering an email already used by someone else isn't allowed. If you test with your own account, be sure to use a different address when building the exploit you'll actually send to the victim.

---

**Reference:**
- https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions

<img width="849" height="674" alt="1" src="https://github.com/user-attachments/assets/ea407ad1-72db-4e20-ac36-820194cdb866" />


---

Chrome applies Lax restrictions by default, so cross-site POST requests carrying cookies get blocked — meaning we need to find a way to trigger the email change via a GET request instead, since the default implementation uses POST:

<img width="738" height="483" alt="2" src="https://github.com/user-attachments/assets/531fb6f5-a194-4602-8eb9-1d50662258c2" />


Switching the method to GET and adding a `_method` parameter turns out to still work:

<img width="739" height="469" alt="3" src="https://github.com/user-attachments/assets/9bd43db7-72c3-4f8a-8e4f-bd0ff7ab8961" />


Final payload:

```
<script>
    document.location = 'https://0a6d00f7038377ec8069495800fd0010.web-security-academy.net/my-account/change-email?email=test4%40test.com&_method=POST';
</script>
```
