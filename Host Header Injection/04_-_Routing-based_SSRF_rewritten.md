# Routing-Based SSRF

This lab is vulnerable to routing-based SSRF through the `Host` header, which can be abused to reach an insecure intranet admin panel sitting on an internal IP address.

**Goal:** Reach the internal admin panel somewhere in the `192.168.0.0/24` range, then delete Carlos.

Note: The Academy's firewall stops the lab environment from reaching arbitrary external systems, so Burp Collaborator's own public server must be used for this to work.

---

**Reference:**
- https://portswigger.net/web-security/host-header/exploiting

pic 1

---

A request to `/` is intercepted, and its `Host` header is swapped for a Burp Collaborator URL:

pic 2

The domain resolves successfully:

pic 3

The request is then sent to Intruder:

pic 4

An admin panel turns out to be reachable at `192.168.0.127`:

pic 5

It initially looks like a POST-only endpoint:

pic 6

But switching to a GET request works just as well:

pic 7
