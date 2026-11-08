# Routing-Based SSRF

This lab is vulnerable to routing-based SSRF through the `Host` header, which can be abused to reach an insecure intranet admin panel sitting on an internal IP address.

**Goal:** Reach the internal admin panel somewhere in the `192.168.0.0/24` range, then delete Carlos.

Note: The Academy's firewall stops the lab environment from reaching arbitrary external systems, so Burp Collaborator's own public server must be used for this to work.

---

**Reference:**
- https://portswigger.net/web-security/host-header/exploiting

<img width="767" height="938" alt="1" src="https://github.com/user-attachments/assets/a864cd4c-99f0-467d-8a06-49d84850b89b" />


---

A request to `/` is intercepted, and its `Host` header is swapped for a Burp Collaborator URL:

<img width="1015" height="399" alt="2" src="https://github.com/user-attachments/assets/812fb284-02a1-4410-ac2e-28170a881db4" />


The domain resolves successfully:

<img width="886" height="108" alt="3" src="https://github.com/user-attachments/assets/7373a569-d9cc-4cd2-bfc2-cc0a06ffaa11" />


The request is then sent to Intruder:

<img width="662" height="386" alt="4" src="https://github.com/user-attachments/assets/a64c1d37-0b76-470a-a3f9-ad93db38030b" />


An admin panel turns out to be reachable at `192.168.0.127`:

<img width="725" height="551" alt="5" src="https://github.com/user-attachments/assets/64991dea-2ae6-4e1d-aa09-9ff2e95457ae" />


It initially looks like a POST-only endpoint:

<img width="696" height="138" alt="6" src="https://github.com/user-attachments/assets/cd932b69-2347-4aa0-ba99-8dc0c51da70e" />


But switching to a GET request works just as well:

<img width="786" height="407" alt="7" src="https://github.com/user-attachments/assets/ebff7ca6-d9e3-49ed-8945-7ce9df7f3dc3" />

