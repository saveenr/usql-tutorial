# Top N Records in group  via RANK, DENSE_RANK or ROW_NUMBER

Many users want to select only TOP n rows per group. This is not possible with the traditional GROUP BY.

You have seen the following example at the beginning of the Ranking functions section. It doesn't show top N records for each partition:

```
@result =
    SELECT
        *,
        ROW_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber,
        RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank,
        DENSE_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank
    FROM @querylog;
```

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

```
@result =
    SELECT
        *,
        DENSE_RANK() 
            OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank
    FROM @querylog;

@result =
    SELECT *
    FROM @result
    WHERE DenseRank <= 3;
```

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


```
@result =
    SELECT
        *,
        RANK() 
            OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank
    FROM @querylog;
    
    @result =
        SELECT *
        FROM @result
        WHERE Rank <= 3;
```

The results:

| **Query** | **Latency** | **Vertical** | **Rank** |
| --- | --- | --- | --- |
| Banana | 300 | Image | 1 |
| Cherry | 300 | Image | 1 |
| Durian | 500 | Image | 3 |
| Apple | 100 | Web | 1 |
| Fig | 200 | Web | 2 |
| Papaya | 200 | Web | 2 |

### TOP N with ROW_NUMBER

```
@result =
    SELECT
        *,
        ROW_NUMBER() 
            OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber
    FROM @querylog;
    
@result =
    SELECT *
    FROM @result
    WHERE RowNumber <= 3;
```

The results:

| **Query** | **Latency** | **Vertical** | **RowNumber** |
| --- | --- | --- | --- |
| Banana | 300 | Image | 1 |
| Cherry | 300 | Image | 2 |
| Durian | 500 | Image | 3 |
| Apple | 100 | Web | 1 |
| Fig | 200 | Web | 2 |
| Papaya | 200 | Web | 3 |

