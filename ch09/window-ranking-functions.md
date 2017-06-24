# Window Ranking Functions

Ranking functions return a ranking value (a long) for each row in each partition as defined by the PARTITION BY and OVER clauses. The ordering of the rank is controlled by the ORDER BY in the OVER clause.

The following are supported ranking functions:

- RANK
- DENSE_RANK
- NTILE
- ROW_NUMBER

Syntax:


```
[RANK() | DENSE_RANK() | ROW_NUMBER() | NTILE(<numgroups>)]
    OVER (
    [PARTITION BY <identifier, > …[n]]
    [ORDER BY <identifier, > …[n] [ASC|DESC]]
    ) AS <alias>
```

The ORDER BY clause is optional for ranking functions. If ORDER BY is specified then it determines the order of the ranking. If ORDER BY is not specified then U-SQL assigns values based on the order it reads record. Thus resulting into non deterministic value of row number, rank or dense rank in the case were order by clause is not specified.

NTILE requires an expression that evaluates to a positive integer. This number specifies the number of groups into which each partition must be divided. This identifier is used only with the NTILE ranking function.

For more details on the OVER clause, see U-SQL reference.

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


## Assign Globally Unique Row Number

It's often useful to assign a globally unique number to each row. This is easy (and more efficient than using a reducer) with the ranking functions.

```
@result =
    SELECT
        *,
        ROW_NUMBER() OVER () AS RowNumber
        FROM @querylog;
```
