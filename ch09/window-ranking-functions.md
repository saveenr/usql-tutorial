# Window Ranking Functions

Ranking functions return a ranking value (a long) for each row in each partition as defined by the PARTITION BY and OVER clauses. The ordering of the rank is controlled by the ORDER BY in the OVER clause.

The following are supported ranking functions:

- RANK
- DENSE\_RANK
- NTILE
- ROW\_NUMBER

Syntax:

[RANK() | DENSE\_RANK() | ROW\_NUMBER() | NTILE(&lt;numgroups&gt;)]

    OVER (

        [PARTITION BY &lt;identifier, &gt; …[n]]

        [ORDER BY &lt;identifier, &gt; …[n] [ASC|DESC]]

) AS &lt;alias&gt;

The ORDER BY clause is optional for ranking functions. If ORDER BY is specified then it determines the order of the ranking. If ORDER BY is not specified then U-SQL assigns values based on the order it reads record. Thus resulting into non deterministic value of row number, rank or dense rank in the case were order by clause is not specified.

NTILE requires an expression that evaluates to a positive integer. This number specifies the number of groups into which each partition must be divided. This identifier is used only with the NTILE ranking function.

For more details on the OVER clause, see U-SQL reference.

ROW\_NUMBER, RANK, and DENSE\_RANK all assign numbers to rows in a window. Rather than cover them separately, it&#39;s more intuitive to see how They respond to the same input.

@result =

SELECT

    \*,

    ROW\_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber,

    RANK()       OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank,

    DENSE\_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank

FROM @querylog;

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

## ROW\_NUMBER

Within each Window (Vertical,either Image or Web), the row number increases by 1 ordered by Latency.

## RANK

Different from ROW\_NUMBER(), RANK() takes into account the value of the Latency which is specified in the ORDER BY clause for the window.

RANK starts with (1,1,3) because the first two values for Latency are the same. Then the next value is 3 because the Latency value has moved on to 500. The key point being that even though duplicate values are given the same rank, the RANK number will &quot;skip&quot; to the next ROW\_NUMBER value. You can see this pattern repeat with the sequence (2,2,4) in the Web vertical.



## DENSE\_RANK

DENSE\_RANK is just like RANK except it doesn&#39;t &quot;skip&quot; to the next ROW\_NUMBER, instead it goes to the next number in the sequence. Notice the sequences (1,1,2) and (2,2,3) in the sample.

Remarks

- If ORDER BY is not specified than ranking function will be applied to rowset without any ordering. This will result into non deterministic behavior on how ranking function is applied
- There is no guarantee that the rows returned by a query using ROW\_NUMBER will be ordered exactly the same with each execution unless the following conditions are true.

- Values of the partitioned column are unique.
- Values of the ORDER BY columns are unique.
- Combinations of values of the partition column and ORDER BY columns are unique.

## NTILE

NTILE distributes the rows in an ordered partition into a specified number of groups. The groups are numbered, starting at one.

The following example splits the set of rows in each partition (vertical) into 4 groups in the order of the query latency, and returns the group number for each row.

The Image vertical has 3 rows, thus it has 3 groups.

The Web vertical has 6 rows, the two extra rows are distributed to the first two groups. That&#39;s why there are 2 rows in group 1 and group 2, and only 1 row in group 3 and group 4.

@result =

    SELECT

        \*,

        NTILE(4) OVER(PARTITION BY Vertical ORDER BY Latency) AS Quartile

    FROM @querylog;

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

NTILE takes a parameter (&quot;numgroups&quot;). Numgroups is a positive int or long constant expression that specifies the number of groups into which each partition must be divided.

If the number of rows in the partition is evenly divisible by numgroups then the groups will have equal size.

If the number of rows in a partition is not divisible by numgroups, this will cause groups of two sizes that differ by one member. Larger groups come before smaller groups in the order specified by the OVER clause.

For example:

- 100 rows divided into 4 groups: [25, 25, 25, 25]
- 102 rows divided into 4 groups: [26, 26, 25, 25]

## Top N Records per Partition via RANK, DENSE\_RANK or ROW\_NUMBER

Many users want to select only TOP n rows per group. This is not possible with the traditional GROUP BY.

You have seen the following example at the beginning of the Ranking functions section. It doesn&#39;t show top N records for each partition:

@result =

SELECT

    \*,

    ROW\_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber,

    RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank,

    DENSE\_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank

FROM @querylog;

The results:

| **Query** | **Latency** | **Vertical** | **Rank** | **DenseRank** | **RowNumber** |
| --- | --- | --- | --- | --- | --- |
| Banana | 300 | Image | 1 | 1 | 1 |
| Cherry | 300 | Image | 1 | 1 | 2 |
| Durian | 500 | Image | 3 | 2 | 3 |
| Apple | 100 | Web | 1 | 1 | 1 |
| Fig | 200 | Web | 2 | 2 | 2 |
| Papaya | 200 | Web | 2 | 2 | 3 |
| Fig | 300 | Web | 4 | 3 | 4 |
| Cherry | 400 | Web | 5 | 4 | 5 |
| Durian | 500 | Web | 6 | 5 | 6 |

### TOP N with DENSE RANK

The following example returns the top 3 records from each group with no gaps in the sequential rank numbering of rows in each windowing partition.

@result =

SELECT

    \*,

    DENSE\_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank

FROM @querylog;

@result =

    SELECT \*

    FROM @result

    WHERE DenseRank &lt;= 3;

The results:

| **Query** | **Latency** | **Vertical** | **DenseRank** |
| --- | --- | --- | --- |
| Banana | 300 | Image | 1 |
| Cherry | 300 | Image | 1 |
| Durian | 500 | Image | 2 |
| Apple | 100 | Web | 1 |
| Fig | 200 | Web | 2 |
| Papaya | 200 | Web | 2 |
| Fig | 300 | Web | 3 |

### TOP N with RANK

@result =

    SELECT

        \*,

        RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank

    FROM @querylog;

@result =

    SELECT \*

    FROM @result

    WHERE Rank &lt;= 3;

The results:

| **Query** | **Latency** | **Vertical** | **Rank** |
| --- | --- | --- | --- |
| Banana | 300 | Image | 1 |
| Cherry | 300 | Image | 1 |
| Durian | 500 | Image | 3 |
| Apple | 100 | Web | 1 |
| Fig | 200 | Web | 2 |
| Papaya | 200 | Web | 2 |

### TOP N with ROW\_NUMBER

@result =

    SELECT

        \*,

        ROW\_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber

    FROM @querylog;

@result =

    SELECT \*

    FROM @result

    WHERE RowNumber &lt;= 3;

The results:

| **Query** | **Latency** | **Vertical** | **RowNumber** |
| --- | --- | --- | --- |
| Banana | 300 | Image | 1 |
| Cherry | 300 | Image | 2 |
| Durian | 500 | Image | 3 |
| Apple | 100 | Web | 1 |
| Fig | 200 | Web | 2 |
| Papaya | 200 | Web | 3 |



## Assign Globally Unique Row Number

It&#39;s often useful to assign a globally unique number to each row. This is easy (and more efficient than using a reducer) with the ranking functions.

@result =

    SELECT

        \*,

        ROW\_NUMBER() OVER () AS RowNumber

    FROM @querylog;



## Getting the Rows that have the maximum (or minimum) value for a Column within a partition

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

To retrieve the row with the minimum value for each partition, in the OVER clause change the **DESC** to **ASC**.