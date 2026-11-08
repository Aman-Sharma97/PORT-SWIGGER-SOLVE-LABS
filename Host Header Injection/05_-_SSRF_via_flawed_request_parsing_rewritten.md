# SSRF via Flawed Request Parsing

This lab is vulnerable to routing-based SSRF because of how it mishandles parsing of a request's intended host. This can be abused to reach an insecure intranet admin panel sitting on an internal IP address.

**Goal:** Reach the internal admin panel somewhere in the `192.168.0.0/24` range, then delete Carlos.

pic 1

Reference: https://portswigger.net/web-security/host-header/exploiting#routing-based-ssrf

pic 2

---

Lab URL: https://0a5100f104ffdc65c3832510004000cf.web-security-academy.net/

If a Collaborator domain is supplied in the `Host` header and a DNS lookup is subsequently received from the target server (or another system in the request path), that confirms requests can likely be routed to arbitrary domains.

Error triggered by the `Host` header on `/product?productId=1`:

pic 3

Adding a second `Host` header instead triggers a gateway error:

pic 4

The request is sent to Intruder:

pic 5

Using `192.168.0.113` produces a 404:

pic 6
pic 7

Repeating the request with the product ID, adding a second `Host` header set to that IP, and switching the path to `/update`:

pic 8

```
GET /admin/ HTTP/2
Host: 192.168.0.113
Host: 0a5100f104ffdc65c3832510004000cf.web-security-academy.net
...
```

This returns the following page:

pic 9

Deleting the user directly doesn't work, so the process was repeated and the response captured instead:

pic 10

A new request is sent to delete the user, assuming the endpoint is `/admin/delete` and the parameter is likely `username`:

pic 11

```
POST /admin/delete HTTP/2
Host: 192.168.0.113
Host: 0a5100f104ffdc65c3832510004000cf.web-security-academy.net
...

username=carlos
```

This returns an error:

pic 12

Repeating the process once more, this time including the `csrf` value from the earlier GET response ("IJTFhUceFHwKZHdbhX2eW0ayNuKksPhv"):

```
POST /admin/delete HTTP/2
Host: 192.168.0.113
Host: 0a5100f104ffdc65c3832510004000cf.web-security-academy.net
...

username=carlos&csrf=IJTFhUceFHwKZHdbhX2eW0ayNuKksPhv
```

pic 13

This produces a 302 redirect:

pic 14

The lab is solved:

pic 15
