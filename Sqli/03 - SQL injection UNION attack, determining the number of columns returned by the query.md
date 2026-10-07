# SQL injection UNION attack, determining the number of columns returned by the query

This lab exposes a SQL injection point in the product category filter.
Because the application's response contains the query results, a
UNION-based SQL injection can be used to add data to the returned result
set.

Before extracting useful information with a UNION attack, the number of
columns in the original query must be identified. This lab demonstrates
that step.

The objective is to make the UNION query return an additional row
containing NULL values and use the response to determine the column
count.

------------------------------------------------------------------------

Reference:
https://portswigger.net/web-security/sql-injection/union-attacks

------------------------------------------------------------------------

The product table displays two visible values: the product name and its
price.

**Pic 1**

First, the original category condition can be terminated with a comment:

``` text
/filter?category=Accessories'--
```

This returns the four products belonging to the Accessories category.

**Pic 2**

The category condition can also be made permanently true:

``` text
/filter?category=Accessories'+or+1=1--
```

To determine the number of columns, a UNION query containing NULL values
can be tested. The following payload shows that the query actually
returns 3 columns rather than 2:

``` text
/filter?category=Accessories'+union+select+NULL,NULL,NULL--
```

**Pic 3**

Instead of NULL values, visible test values can also be supplied to
identify the positions of the returned columns:

``` text
/filter?category=Accessories'+union+all+select+'0','1','2'--
```

**Pic 4**
