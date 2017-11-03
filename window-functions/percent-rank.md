# PERCENT\_RANK

PERCENT\_RANK calculates the relative rank of a row within a group of rows. PERCENT\_RANK is used to evaluate the relative standing of a value within a rowset or partition. The range of values returned by PERCENT\_RANK is in the range `0 <= x <= 1`. 


```
PERCENT_RANK( )
    OVER (
        [PARTITION BY <identifier>; …[n]]
        ORDER BY <identifier,> …[n] [ASC|DESC]
    ) AS <alias>;
```

Notes

* The first row in any set has a PERCENT\_RANK of 0.
* NULL values are treated as the lowest possible values.
* PERCENT\_RANK is similar to the CUME\_DIST the function. Unlike CUME\_DIST, PERCENT\_RANK is always 0 for the first row.

The following example uses the PERCENT\_RANK function to compute the latency percentile for each query within a vertical. The PARTITION BY clause is specified to partition the rows in the result set by the vertical. The ORDER BY clause in the OVER clause orders the rows in each partition. The value returned by the PERCENT\_RANK function represents the rank of the queries' latency within a vertical as a percentage.

```
@result=
    SELECT
        *,
        PERCENT_RANK() 
            OVER (PARTITION BY Vertical ORDER BY Latency) AS PercentRank
    FROM @querylog;
```



| **Query** | **Latency:int** | **Vertical** | **PercentRank** |
| --- | --- | --- | --- |
| Banana | 300 | Image | 0 |
| Cherry | 300 | Image | 0 |
| Durian | 500 | Image | 1 |
| Apple | 100 | Web | 0 |
| Fig | 200 | Web | 0.2 |
| Papaya | 200 | Web | 0.2 |
| Fig | 300 | Web | 0.6 |
| Cherry | 400 | Web | 0.8 |
| Durian | 500 | Web | 1 |



