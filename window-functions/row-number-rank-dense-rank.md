# ROW_NUMBER, RANK, and DENSE_RANK

ROW_NUMBER, RANK, and DENSE_RANK all assign numbers to rows in a window. Rather than cover them separately, it's more intuitive to see how They respond to the same input.

```
@result =
SELECT
*,
ROW_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber,
RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank,
DENSE_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank
FROM @querylog;
```

Note the OVER clauses are identical. The result:

| **Query** | **Latency:int** | **Vertical** | **RowNumber** | **Rank** | **DenseRank** |
| --- | --- | --- | --- | --- | --- |
| Banana | 300 | Image | 1 | 1 | 1 |
| Cherry | 300 | Image | 2 | 1 | 1 |
| Durian | 500 | Image | 3 | 3 | 2 |
| Apple | 100 | Web | 1 | 1 | 1 |
| Fig | 200 | Web | 2 | 2 | 2 |
| Papaya | 200 | Web | 3 | 2 | 2 |
| Fig | 300 | Web | 4 | 4 | 3 |
| Cherry | 400 | Web | 5 | 5 | 4 |
| Durian | 500 | Web | 6 | 6 | 5 |

## ROW_NUMBER

Within each Window (Vertical,either Image or Web), the row number increases by 1 ordered by Latency.

## RANK

Different from ROW_NUMBER(), RANK() takes into account the value of the Latency which is specified in the ORDER BY clause for the window.

RANK starts with (1,1,3) because the first two values for Latency are the same. Then the next value is 3 because the Latency value has moved on to 500. The key point being that even though duplicate values are given the same rank, the RANK number will "skip" to the next ROW_NUMBER value. You can see this pattern repeat with the sequence (2,2,4) in the Web vertical.



## DENSE_RANK

DENSE_RANK is just like RANK except it doesn't "skip" to the next ROW_NUMBER, instead it goes to the next number in the sequence. Notice the sequences (1,1,2) and (2,2,3) in the sample.

Remarks

- If ORDER BY is not specified than ranking function will be applied to rowset without any ordering. This will result into non deterministic behavior on how ranking function is applied
- There is no guarantee that the rows returned by a query using ROW_NUMBER will be ordered exactly the same with each execution unless the following conditions are true.

- Values of the partitioned column are unique.
- Values of the ORDER BY columns are unique.
- Combinations of values of the partition column and ORDER BY columns are unique.
