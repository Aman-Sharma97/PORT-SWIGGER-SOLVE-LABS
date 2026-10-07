# SQL injection UNION attack, retrieving data from other tables

This lab demonstrates how a UNION SQL injection can be used to retrieve
information from a different database table.

The database contains a `users` table with `username` and `password`
columns. The objective is to extract those values and use the
administrator's credentials to log in.

------------------------------------------------------------------------

Reference:
https://portswigger.net/web-security/sql-injection/union-attacks

------------------------------------------------------------------------

The response contains two visible values: the post description and its
content.

**Pic 1**

The category parameter can first be tested with these payloads. The
first returns the products from Gifts, while the second removes the
category restriction:

``` text
/filter?category=Gifts'--
/filter?category=Gifts'+or+1=1--
```

The response indicates that the original query returns two columns, so a
two-column UNION query can be used:

``` text
/filter?category=Gifts'+union+all+select+NULL,NULL--
```

**Pic 2**

Because the table and column names are known, the `username` and
`password` fields can be selected directly from the `users` table:

``` text
/filter?category=Gifts'+union+all+select+username,password+from+users--
```

This exposes the usernames and passwords returned by the database.

**Pic 3**
