# SQL injection UNION attack, finding a column containing text

This lab uses the same UNION-based SQL injection technique as the
previous exercise. After identifying the number of columns, the next
task is to determine which column can hold string data.

The lab provides a random value that must be made visible in the
application's response. Finding the compatible column is necessary
before using UNION to extract textual information.

------------------------------------------------------------------------

Reference:
https://portswigger.net/web-security/sql-injection/union-attacks

------------------------------------------------------------------------

The response displays two product values: the product name and the
price.

**Pic 1**

The following payloads confirm that the category filter can be modified
and that all products can also be returned:

``` text
/filter?category=Accessories'--
/filter?category=Accessories'+or+1=1--
```

The earlier column-count test shows that the underlying query returns 3
columns. Therefore, a UNION query with three values can be used:

``` text
/filter?category=Accessories'+union+all+select+NULL,NULL,NULL--
```

The next step is to place the supplied string in one column at a time.
Here, the value `Qrc0Pq` is inserted into the second column:

``` text
/filter?category=Accessories'+union+all+select+'0','Qrc0Pq','1234'--
```

If the supplied string appears in the response, that column is
compatible with string data.

**Pic 2**
