## Filtering on Aggregated Rows

We’ll start again with a simple GROUP BY

```
@output =
  SELECT
    Region,
    SUM(Duration) AS TotalDuration
  FROM @searchlog
  GROUP BY Region;
```

| en_ca | 24 |
| --- | --- |
| en_ch | 10 |
| en_fr | 241 |
| en_gb | 688 |
| en_gr | 305 |
| en_mx | 422 |
| en_us | 8291 |

## Filtering with WHERE

You might try `WHERE` here.

```
@output =
  SELECT  
    Region,  
    SUM(Duration) AS TotalDuration  
  FROM @searchlog  
  WHERE TotalDuration > 200  
  GROUP BY Region;
```

Which will cause an error. Because `WHERE` can only work on the input columns to the statement, not the output columns

We could use multiple U-SQL Statements to accomplish this

```
@output =  
  SELECT  
    Region,  
    SUM(Duration) AS TotalDuration  
  FROM @searchlog  
  GROUP BY Region;  

@output =  
  SELECT *  
  FROM @output  
  WHERE TotalDuration > 200;
```

## Filtering with HAVING

Alternatively , we can use the HAVING clause which is designed to filter columns when a GROUP BY is used..

```
@output =  
  SELECT  
    Region,  
    SUM(Duration) AS TotalDuration  
  FROM @searchlog  
  GROUP BY Region  
  HAVING SUM(Duration) > 200;
```

You may have noticed that `SUM(Duration)` was repeated in the HAVING clause. That’s because HAVING (like WHERE) cannot use columns created in the SELECT clause.


