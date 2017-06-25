## PERCENTILE\_CONT & PERCENTILE\_DISC

These two functions calculates a percentile based on a continuous or discrete distribution of the column values.

Syntax

```
[PERCENTILE_CONT | PERCENTILE_DISC] ( numeric_literal )
    WITHIN GROUP ( ORDER BY <identifier> [ASC | DESC] )
    OVER ( [PARTITION BY <identifier,>…[n] ] ) AS <alias>
```

numeric\_literal - The percentile to compute. The value must range between 0.0 and 1.0.

WITHIN GROUP \( ORDER BY \[ASC \| DESC\]\) - Specifies a list of numeric values to sort and compute the percentile over. Only one column identifier is allowed. The expression must evaluate to a numeric type. Other data types are not allowed. The default sort order is ascending.

OVER \(\[PARTITION BY …\[n\] \] \) - Divides the input rowset into partitions as per the partition key to which the percentile function is applied. For more information, see RANKING section of this document. Note: Any nulls in the data set are ignored.

PERCENTILE\_CONT calculates a percentile based on a continuous distribution of the column value. The result is interpolated and might not be equal to any of the specific values in the column.

PERCENTILE\_DISC calculates the percentile based on a discrete distribution of the column values. The result is equal to a specific value in the column. In other words, PERCENTILE\_DISC, in contrast to PERCENTILE\_CONT, always returns an actual \(original input\) value.

You can see how both work in the example below which tries to find the median \(percentile=0.50\) value for Latency within each Vertical

```
@result =
    SELECT
        Vertical,
        Query,
        PERCENTILE_CONT(0.5) 
            WITHIN GROUP (ORDER BY Latency)
            OVER ( PARTITION BY Vertical ) AS PercentileCont50,
        PERCENTILE_DISC(0.5)
            WITHIN GROUP (ORDER BY Latency)
            OVER ( PARTITION BY Vertical ) AS PercentileDisc50
    FROM @querylog;
```

The results:

| **Query** | **Latency** | **Vertical** | **PercentileCont50** | **PercentilDisc50** |
| :--- | :--- | :--- | :--- | :--- |
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

