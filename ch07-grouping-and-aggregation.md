# Grouping and Aggregation

Grouping, in essence, collapses multiple rows into single rows based on  
some criteria. Hand-in-hand with performing a grouping operation, some  
fields in the output rowset must be aggregated into some meaningful  
value \(or discarded if no possible or meaningful aggregation can be  
done\).

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

Now, let's add all the numbers together. This yields a rowset with  
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
Region.

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

This is a good opportunity to explore a common use of the **HAVING**  
operator. We can use **HAVING** to restrict the output rowset to those  
rows that have aggregate values we are interested in. For example,  
perhaps we want to find all the Regions where total dwell time is above  
some value.

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
    COUNT\(\) AS NumSessions,
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

Count the number of total sessions by Region and include total duration  
for that language.

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

## A Note: Data types Coming from Aggregations

You should be aware of how some aggregation operators deal with data  
types.

For example, the input data type is double:

* SUM\(double\) -&gt; double
* COUNT\(double\) -&gt; long\(int64\)

But if the input data type is numeric \(long/int/short/byte, etc.\):

* SUM\(type\) -&gt; long\(int64\)
* COUNT\(type\) -&gt; long\(int64\)

## Where You Can Use Aggregates in a Query

Aggregates can ONLY appear in a SELECT clause.

## DISTINCT with Aggregates

Every aggregate function can take a **DISTINCT** qualifier.

For example

`COUNT(DISTINCT x)`

**DISTINCT** also works for user-defined aggregates.

MyAggregator\(DISTINCT x,y,z\)

## Filtering on Aggregated Values

We’ll start again with a simply **GROUP BY**

```
@output =
  SELECT
    Region,
    SUM(Duration) AS TotalDuration
  FROM searchlog
  GROUP BY Region;
```

| en\_ca | 24 |
| --- | --- |
| en\_ch | 10 |
| en\_fr | 241 |
| en\_gb | 688 |
| en\_gr | 305 |
| en\_mx | 422 |
| en\_us | 8291 |

You might try WHERE here.

```
@output =
  SELECT  
    Region,  
    SUM\(Duration\) AS TotalDuration  
  FROM @searchlog  
  WHERE TotalDuration &gt; 200  
  GROUP BY Region;
```

Which will cause an error. Because WHERE can only work on the input  
columns to the statement, not the output columns

We could of course, use multiple U-SQL Statements to accomplish this

```
@output =  
  SELECT  
    Region,  
    SUM(Duration) AS TotalDuration  
  FROM @searchlog  
  GROUP BY Region;  

@output2 =  
  SELECT *  
  FROM @output  
  WHERE TotalDuration &gt; 200;
```

Alternatively , we can use the HAVING clause which is designed to filter  
columns when a GROUP BY is used..

```
@output =  
  SELECT  
    Region,  
    SUM(Duration) AS TotalDuration  
  FROM @searchlog  
  GROUP BY Region  
  HAVING SUM(Duration) &gt; 200;
```

You may have noticed that “SUM\(Duration\)” was repeated in the HAVING  
clause. That’s because HAVING \(like WHERE\) cannot use columns created in  
the SELECT clause.

If you try the code below, it will be a compilation error.

```
@output =  
  SELECT  
    Region,  
    SUM(Duration) AS TotalDuration  
  FROM @searchlog  
  GROUP BY Region  
  HAVING TotalDuration &gt; 200;
```

## 



