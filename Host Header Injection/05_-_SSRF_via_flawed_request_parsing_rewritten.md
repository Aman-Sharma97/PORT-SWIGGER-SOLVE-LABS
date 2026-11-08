# SSRF via Flawed Request Parsing

This lab is vulnerable to routing-based SSRF because of how it mishandles parsing of a request's intended host. This can be abused to reach an insecure intranet admin panel sitting on an internal IP address.

**Goal:** Reach the internal admin panel somewhere in the `192.168.0.0/24` range, then delete Carlos.

<img width="784" height="468" alt="1" src="https://github.com/user-attachments/assets/71c8a2ee-4e78-4796-955d-016d4edf8129" />


Reference: https://portswigger.net/web-security/host-header/exploiting#routing-based-ssrf

<img width="848" height="917" alt="2" src="https://github.com/user-attachments/assets/4d28a7ba-ec65-49e9-8157-2c090d99ccd3" />


---

Lab URL: https://0a5100f104ffdc65c3832510004000cf.web-security-academy.net/

If a Collaborator domain is supplied in the `Host` header and a DNS lookup is subsequently received from the target server (or another system in the request path), that confirms requests can likely be routed to arbitrary domains.

Error triggered by the `Host` header on `/product?productId=1`:

<img width="1160" height="318" alt="3" src="https://github.com/user-attachments/assets/2ff96b7f-ef8c-48c2-a8b5-ee8a5419ec29" />


Adding a second `Host` header instead triggers a gateway error:

<img width="1286" height="367" alt="4" src="https://github.com/user-attachments/assets/17390ff8-98fb-427c-b23e-6e056477adba" />


The request is sent to Intruder:

<img width="817" height="407" alt="5" src="https://github.com/user-attachments/assets/079d4930-02e8-48f3-9572-aae0c57709f5" />


Using `192.168.0.113` produces a 404:

<img width="768" height="347" alt="6" src="https://github.com/user-attachments/assets/9cc31356-f0d8-4f66-a2b4-912d7d8a893a" />

<img width="690" height="537" alt="7" src="https://github.com/user-attachments/assets/791a6104-1535-4db6-9a90-dccecf37a24a" />


Repeating the request with the product ID, adding a second `Host` header set to that IP, and switching the path to `/update`:

<img width="943" height="360" alt="8" src="https://github.com/user-attachments/assets/8aafd806-fbba-43ec-8dd8-7ee0046bab10" />


```
GET /admin/ HTTP/2
Host: 192.168.0.113
Host: 0a5100f104ffdc65c3832510004000cf.web-security-academy.net
...
```

This returns the following page:

<img width="886" height="419" alt="9" src="https://github.com/user-attachments/assets/f0891d09-6692-40f6-9151-e9b8516e5ea6" />


Deleting the user directly doesn't work, so the process was repeated and the response captured instead:

<img width="703" height="152" alt="10" src="https://github.com/user-attachments/assets/bd615402-8209-4d36-801d-2bb41fa75916" />


A new request is sent to delete the user, assuming the endpoint is `/admin/delete` and the parameter is likely `username`:

<img width="786" height="321" alt="11" src="https://github.com/user-attachments/assets/2d73c91e-da2d-416f-9a16-cb084cdc5a47" />


```
POST /admin/delete HTTP/2
Host: 192.168.0.113
Host: 0a5100f104ffdc65c3832510004000cf.web-security-academy.net
...

username=carlos
```

This returns an error:

<img width="399" height="132" alt="12" src="https://github.com/user-attachments/assets/764c9833-dff2-408c-b3f7-64d2c1fad58e" />


Repeating the process once more, this time including the `csrf` value from the earlier GET response ("IJTFhUceFHwKZHdbhX2eW0ayNuKksPhv"):

```
POST /admin/delete HTTP/2
Host: 192.168.0.113
Host: 0a5100f104ffdc65c3832510004000cf.web-security-academy.net
...

username=carlos&csrf=IJTFhUceFHwKZHdbhX2eW0ayNuKksPhv
```

<img width="943" height="387" alt="13" src="https://github.com/user-attachments/assets/b8958320-688b-4a7d-a284-250082983cb1" />


This produces a 302 redirect:

<img width="251" height="154" alt="14" src="https://github.com/user-attachments/assets/e5d38aab-0c17-417b-a977-2c10566fac94" />


The lab is solved:

<img width="643" height="91" alt="15" src="https://github.com/user-attachments/assets/c6ee6174-2c57-420c-ba2a-911859d0c989" />

