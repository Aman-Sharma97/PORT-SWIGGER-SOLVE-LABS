# User ID Controlled by a Request Parameter — With Unpredictable User IDs

The account page in this lab has a horizontal privilege escalation flaw, but users are identified by GUIDs instead of simple usernames.

**Goal:** Find `carlos`'s GUID, then retrieve and submit his API key.

Login credentials for your own account: wiener:peter

---

**Reference:**
- https://portswigger.net/web-security/access-control

pic 1

---

Clicking "Home" sends this GET request:

pic 2

The administrator's GUID can be found by inspecting the blog posts:

pic 3

That GUID is then used to craft a new request:

pic 4
