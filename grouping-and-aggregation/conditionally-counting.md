# Conditionally counting

Consider the following RowSet:

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso",    1500.0),
            ("Woodgrove",  2700.0),
            ((string)null, 6700.0)
        ) AS D( customer, amount );
```

We can easily count the number of rows:

```
@b =
    SELECT 
       COUNT() AS Count1
       FROM @a;
```

But what if we wanted to count the rows only if they met a certain criteria? We can accomplish this my using the SUM operator with an expression that will return either a 1 or 0.

```
@b =
    SELECT 
       COUNT() AS Count,
       SUM(customer.Contains("o") ? 1 : 0) AS CountContainsLowercaseO,
       SUM(customer==null ? 1: 0) AS CountIsNull
    FROM @a;
```

| Count | CountContainsLowercaseOnt2 | CountIsNull |
| --- | --- | --- |
| 3 | 2 | 1 |



