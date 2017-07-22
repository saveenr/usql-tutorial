# Any N Records per group

Just as you sometimes want to get the "TOP n rows per group". Sometimes you just want "ANY n rows per group".

For example, "list any 10 customers per zipcode".


```
@result =
    SELECT
        *,
        ROW_NUMBER() 
            OVER (PARTITION BY ZipCode ORDER BY 1) AS RowNumber
    FROM @customers;
    
@result =
    SELECT *
    FROM @result
    WHERE RowNumber <= 10;
```

This technique makes use of the "ORDER BY 1" pattern - which effectively means no ordering is performed.
