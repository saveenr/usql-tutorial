
# Analytic functions

Analytic functions are used to understand the distributions of values in windows. The most common scenario for using analytic functions is the computation of percentiles.

Supported analytic window functions

- CUME\_DIST
- PERCENT\_RANK
- PERCENTILE\_CONT
- PERCENTILE\_DISC
- CUME\_DIST

## CUME\_DIST

CUME\_DIST computes the relative position of a specified value in a group of values. It calculates the percent of queries that have a latency less than or equal to the current query latency in the same vertical. For a row R, assuming ascending ordering, the CUME\_DIST of R is the number of rows with values lower than or equal to the value of R, divided by the number of rows evaluated in the partition or query result set. CUME\_DIST returns numbers in the range `0 < x <= 1`

```
CUME\_DIST()
    OVER (
        [PARTITION BY <identifier, > …[n]]
        ORDER BY >identifier, > …[n] [ASC|DESC]
) AS <alias>
```

The following example uses the CUME\_DIST function to compute the latency percentile for each query within a vertical.

```
@result=
    SELECT
        \*,
        CUME\_DIST() OVER(PARTITION BY Vertical ORDER BY Latency) AS CumeDist
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

- There are 6 rows in the partition where partition key is &quot;Web&quot; (4th row and down)
- There are 6 rows with the value equal or lower than 500, so the CUME\_DIST equals to 6/6=1
- There are 5 rows with the value equal or lower than 400, so the CUME\_DIST equals to 5/6=0.83
- There are 4 rows with the value equal or lower than 300, so the CUME\_DIST equals to 4/6=0.66
- There are 3 rows with the value equal or lower than 200, so the CUME\_DIST equals to 3/6=0.5. There are two rows with the same latency value.
- There is 1 row with the value equal or lower than 100, so the CUME\_DIST equals to 1/6=0.16.

Usage notes:

- Tie values always evaluate to the same cumulative distribution value.
- NULL values are treated as the lowest possible values.
- You must specify the ORDER BY clause to calculate CUME\_DIST.
- CUME\_DIST is similar to the PERCENT\_RANK function
- Note: The ORDER BY clause is not allowed if the SELECT statement is not followed by OUTPUT. Thus ORDER BY clause in the OUTPUT statement determines the display order of the resultant rowset.

## PERCENT\_RANK

PERCENT\_RANK calculates the relative rank of a row within a group of rows. PERCENT\_RANK is used to evaluate the relative standing of a value within a rowset or partition. The range of values returned by PERCENT\_RANK is greater than 0 and less than or equal to 1. Unlike CUME\_DIST, PERCENT\_RANK is always 0 for the first row.


```
PERCENT_RANK()
    OVER (

        [PARTITION BY <identifier>; …[n]]
        ORDER BY <identifier,> …[n] [ASC|DESC]
    ) AS <alias>;
```

Notes

- The first row in any set has a PERCENT\_RANK of 0.
- NULL values are treated as the lowest possible values.
- You must specify the ORDER BY clause to calculate PERCENT\_RANK.
- CUME\_DIST is similar to the PERCENT\_RANK function
- The following example uses the PERCENT\_RANK function to compute the latency percentile for each query within a vertical.
- The PARTITION BY clause is specified to partition the rows in the result set by the vertical. The ORDER BY clause in the OVER clause orders the rows in each partition.
- The value returned by the PERCENT\_RANK function represents the rank of the queries&#39; latency within a vertical as a percentage.

```
@result=
    SELECT
        \*,
        PERCENT\_RANK() OVER(PARTITION BY Vertical ORDER BY Latency) AS PercentRank
    FROM @querylog;
```

The results:

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

## PERCENTILE\_CONT &amp; PERCENTILE\_DISC

These two functions calculates a percentile based on a continuous or discrete distribution of the column values.

Syntax

```
[PERCENTILE\_CONT | PERCENTILE\_DISC] ( numeric\_literal )
    WITHIN GROUP ( ORDER BY &lt;identifier&gt; [ASC | DESC] )
    OVER ( [PARTITION BY &lt;identifier,&gt;…[n] ] ) AS &lt;alias&gt;
```

numeric\_literal - The percentile to compute. The value must range between 0.0 and 1.0.

WITHIN GROUP ( ORDER BY [ASC | DESC]) - Specifies a list of numeric values to sort and compute the percentile over. Only one column identifier is allowed. The expression must evaluate to a numeric type. Other data types are not allowed. The default sort order is ascending.

OVER ([PARTITION BY …[n] ] ) - Divides the input rowset into partitions as per the partition key to which the percentile function is applied. For more information, see RANKING section of this document. Note: Any nulls in the data set are ignored.

PERCENTILE\_CONT calculates a percentile based on a continuous distribution of the column value. The result is interpolated and might not be equal to any of the specific values in the column.

PERCENTILE\_DISC calculates the percentile based on a discrete distribution of the column values. The result is equal to a specific value in the column. In other words, PERCENTILE\_DISC, in contrast to PERCENTILE\_CONT, always returns an actual (original input) value.

You can see how both work in the example below which tries to find the median (percentile=0.50) value for Latency within each Vertical

```
@result =
    SELECT
        Vertical,
        Query,
        PERCENTILE\_CONT(0.5)
            WITHIN GROUP (ORDER BY Latency)
            OVER ( PARTITION BY Vertical ) AS PercentileCont50,
        PERCENTILE\_DISC(0.5)
            WITHIN GROUP (ORDER BY Latency)
            OVER ( PARTITION BY Vertical ) AS PercentileDisc50
    FROM @querylog;
```

The results:

| **Query** | **Latency** | **Vertical** | **PercentileCont50** | **PercentilDisc50** |
| --- | --- | --- | --- | --- |
| Banana | 300 | Image | 300 | 300 |
| Cherry | 300 | Image | 300 | 300 |
| Durian | 500 | Image | 300 | 300 |
| Apple | 100 | Web | 250 | 200 |
| Fig | 200 | Web | 250 | 200 |
| Papaya | 200 | Web | 250 | 200 |
| Fig | 300 | Web | 250 | 200 |
| Cherry | 400 | Web | 250 | 200 |
| Durian | 500 | Web | 250 | 200 |

For PERCENTILE\_CONT because values can be interpolated, the median for web is 250 even though no query in the web vertical had a latency of 250.

PERCENTILE\_DISC does not interpolate values, so the median for Web is 200 - which is an actual value found in the input rows.

