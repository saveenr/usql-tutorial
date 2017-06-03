# Grouping and Aggregation

Grouping, in essence, collapses multiple rows into single rows based on some criteria. Hand-in-hand with performing a grouping operation, some fields in the output rowset must be aggregated into some meaningful value \(or discarded if no possible or meaningful aggregation can be done\).

We can witness this behavior by building up to it in stages.

```
// list all session durations.  
@output =  
  SELECT Duration  
  FROM @searchlog;
```

This creates a simple list of integers.

| Duration |
| --- |
| 73 |
| 614 |
| 74 |
| 24 |
| 1213 |
| 241 |
| 502 |
| 60 |
| 1270 |
| 610 |
| 422 |
| 283 |
| 305 |
| 10 |
| 612 |
| 1220 |
| 691 |
| 63 |
| 30 |
| 119 |
| 732 |
| 183 |
| 630 |

Now, let's add all the numbers together. This yields a RowSet with  
exactly one row and one column.

```
// Find the total duration for all sessions combined
@output =  
  SELECT  
    SUM(Duration) AS TotalDuration  
  FROM @searchlog;
```

| Duration |
| --- |
| 9981 |

Now let's use the **GROUP BY** operator to break apart the totals by  
`Region`.

```
// find the total Duration by Region
@output =
  SELECT
    Region,
    SUM(Duration) AS TotalDuration
  FROM searchlog
  GROUP BY Region;
```

This returns:

| en\_ca | 24 |
| --- | --- |
| en\_ch | 10 |
| en\_fr | 241 |
| en\_gb | 688 |
| en\_gr | 305 |
| en\_mx | 422 |
| en\_us | 8291 |

This is a good opportunity to explore a common use of the **HAVING **operator. We can use **HAVING** to restrict the output RowSet to those rows that have aggregate values we are interested in. For example, perhaps we want to find all the Regions where total dwell time is above some value.

```
// find all the Regions where the total dwell time is &gt; 200
@output =
  SELECT
    Region,
    SUM(Duration) AS TotalDuration
  FROM @searchlog
  GROUP BY Region
  HAVING TotalDuration &gt; 200;
```

| en-fr | 241 |
| --- | --- |
| en-gb | 688 |
| en-gr | 305 |
| en-mx | 422 |
| en-us | 8291 |

```
// Count the number of total sessions.
@output =
  SELECT
    COUNT() AS NumSessions
  FROM @searchlog;
```

| 23 |
| --- |


Count the number of total sessions by Region.

```
@output =
  SELECT
    COUNT() AS NumSessions,
    Region
  FROM @searchlog
  GROUP BY Region;
```

| 1 | en\_ca |
| --- | --- |
| 1 | en\_ch |
| 1 | en\_fr |
| 2 | en\_gb |
| 1 | en\_gr |
| 1 | en\_mx |
| 16 | en\_us |

Count the number of total sessions by Region and include total duration for that language.

```
@output =
  SELECT
    COUNT() AS NumSessions,
    Region,
    SUM(Duration) AS TotalDuration,
    AVG(Duration) AS AvgDwellTtime,
    MAX(Duration) AS MaxDuration,
    MIN(Duration) AS MinDuration
    FROM @searchlog
  GROUP BY Region;
```

| NumSessions | Region | TotalDuration | AvgDuration | MaxDuration | MinDuration |
| --- | --- | --- | --- | --- | --- |
| 1 | en\_ca | 24 | 24 | 24 | 24 |
| 1 | en\_ch | 10 | 10 | 10 | 10 |
| 1 | en\_fr | 241 | 241 | 241 | 241 |
| 2 | en\_gb | 688 | 344 | 614 | 74 |
| 1 | en\_gr | 305 | 305 | 305 | 305 |
| 1 | en\_mx | 422 | 422 | 422 | 422 |
| 16 | en\_us | 8291 | 518.1875 | 1270 | 30 |

## Data types coming from aggregate functions

You should be aware of how some aggregation operators deal with data types. Some aggregations will promote a numeric type to a "larger" type. Other aggregations may switch types entirely.

For example, if the input data type is double:

* `SUM(double)` -&gt; double
* `COUNT(double)` -&gt; long \(a.k.a int64\)

If the input data type is any numeric \(long/int/short/byte, etc.\):

* `SUM(byte)` -&gt; long \(a.k.a int64\)
* `COUNT(int)` -&gt; long \(a.k.a int64\)

## Notes

Aggregates can ONLY appear in a `SELECT` clause.

## DISTINCT with Aggregates

Every aggregate function can take a `DISTINCT` qualifier.

For example

`COUNT(DISTINCT x)`

**DISTINCT** also works for user-defined aggregates.

`MyAggregator(DISTINCT x,y,z)`

## 



