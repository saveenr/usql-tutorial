## NTILE

NTILE distributes the rows in an ordered partition into a specified number of groups. The groups are numbered, starting at one.

The following example splits the set of rows in each partition (vertical) into 4 groups in the order of the query latency, and returns the group number for each row.

The Image vertical has 3 rows, thus it has 3 groups.

The Web vertical has 6 rows, the two extra rows are distributed to the first two groups. That's why there are 2 rows in group 1 and group 2, and only 1 row in group 3 and group 4.


```
@result =
SELECT
*,
NTILE(4) OVER(PARTITION BY Vertical ORDER BY Latency) AS Quartile
FROM @querylog;
```

The results:

| **Query** | **Latency** | **Vertical** | **Quartile** |
| --- | --- | --- | --- |
| Banana | 300 | Image | 1 |
| Cherry | 300 | Image | 2 |
| Durian | 500 | Image | 3 |
| Apple | 100 | Web | 1 |
| Fig | 200 | Web | 1 |
| Papaya | 200 | Web | 2 |
| Fig | 300 | Web | 2 |
| Cherry | 400 | Web | 3 |
| Durian | 500 | Web | 4 |

NTILE takes a parameter ("numgroups"). Numgroups is a positive int or long constant expression that specifies the number of groups into which each partition must be divided.

If the number of rows in the partition is evenly divisible by numgroups then the groups will have equal size.

If the number of rows in a partition is not divisible by numgroups, this will cause groups of two sizes that differ by one member. Larger groups come before smaller groups in the order specified by the OVER clause.

For example:

- 100 rows divided into 4 groups: [25, 25, 25, 25]
- 102 rows divided into 4 groups: [26, 26, 25, 25]

