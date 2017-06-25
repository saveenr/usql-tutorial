## CUME\_DIST

**CUME\_DIST** computes the relative position of a specified value in a group of values.

For a column "X", It calculates the percent of rows that have an X less than or equal to the current X in the same window.

For a row R, assuming ascending ordering, the **CUME\_DIST** of R is the number of rows with values lower than or equal to the value of R, divided by the number of rows evaluated in the partition or query result set. **CUME\_DIST** returns numbers in the range `0 < x <= 1`

```
CUME_DIST()
    OVER (
        [PARTITION BY <identifier, > …[n]]
        ORDER BY >identifier, > …[n] [ASC|DESC]
    ) AS <alias>
```

The following example uses the CUME\_DIST function to compute the latency percentile for each query within a vertical.

```
@result=
    SELECT
        *,
        CUME_DIST() OVER(PARTITION BY Vertical ORDER BY Latency) AS CumeDist
    FROM @querylog;
```

The results:

| **Query** | **Latency** | **Vertical** | **CumeDist** |
| --- | --- | --- | --- |
| Durian | 500 | Image | 1 |
| Banana | 300 | Image | 0.666666666666667 |
| Cherry | 300 | Image | 0.666666666666667 |
| Durian | 500 | Web | 1 |
| Cherry | 400 | Web | 0.833333333333333 |
| Fig | 300 | Web | 0.666666666666667 |
| Fig | 200 | Web | 0.5 |
| Papaya | 200 | Web | 0.5 |
| Apple | 100 | Web | 0.166666666666667 |

* There are 6 rows in the partition where partition key is "Web" \(4th row and down\)
* There are 6 rows with the value equal or lower than 500, so the CUME\_DIST equals to 6/6=1
* There are 5 rows with the value equal or lower than 400, so the CUME\_DIST equals to 5/6=0.83
* There are 4 rows with the value equal or lower than 300, so the CUME\_DIST equals to 4/6=0.66
* There are 3 rows with the value equal or lower than 200, so the CUME\_DIST equals to 3/6=0.5. There are two rows with the same latency value.
* There is 1 row with the value equal or lower than 100, so the CUME\_DIST equals to 1/6=0.16.

Usage notes:

* Tie values always evaluate to the same cumulative distribution value.
* NULL values are treated as the lowest possible values.
* You must specify the ORDER BY clause to calculate CUME\_DIST.
* CUME\_DIST is similar to the PERCENT\_RANK function
* Note: The ORDER BY clause is not allowed if the SELECT statement is not followed by OUTPUT. Thus ORDER BY clause in the OUTPUT statement determines the display order of the resultant rowset.



