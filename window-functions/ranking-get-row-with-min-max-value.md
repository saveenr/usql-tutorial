# Getting the Rows that have the maximum (or minimum) value for a Column within a partition

Another scenario easily done through the ranking functions, is finding the row that contains the max value in a partition

Returning to our original input data set, imagine we want to partition by Vertical and within each vertical find the row that has the maximum value for latency.

| **Query** | **Latency** | **Vertical** |
| --- | --- | --- |
| Banana | 300 | Image |
| Cherry | 300 | Image |
| Durian | 500 | Image |
| Apple | 100 | Web |
| Fig | 200 | Web |
| Papaya | 200 | Web |
| Fig | 300 | Web |
| Cherry | 400 | Web |
| Durian | 500 | Web |

The desired output for is as follows. As clearly 500 is the maximum latency in both Image and Web

| **Query** | **Latency** | **Vertical** |
| --- | --- | --- |
| Durian | 500 | Image |
| Durian | 500 | Web |

The U-SQL that accomplishes this uses ROW\_NUMBER

```
@results =
     SELECT
         Query,
         Latency,
         Vertical,
         ROW\_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency DESC) AS rn
     FROM @querylog;

@results =
     SELECT
         Query,
         Latency,
         Vertical
     FROM @results
     WHERE rn==1;
```

To retrieve the row with the minimum value for each partition, in the OVER clause change the **DESC** to **ASC**.