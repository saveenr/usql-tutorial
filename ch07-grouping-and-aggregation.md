Grouping, in essence, collapses multiple rows into single rows based on  
some criteria. Hand-in-hand with performing a grouping operation, some  
fields in the output rowset must be aggregated into some meaningful  
value \(or discarded if no possible or meaningful aggregation can be  
done\).

We can witness this behavior by building up to it in stages.

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

// Find the total duration for all sessions combined
```

@output =  
  SELECT  
   SUM\(Duration\) AS TotalDuration  
   FROM @searchlog;

```
| 9981 |
| --- |


Now let's use the **GROUP BY** operator to break apart the totals by  
Region.

// find the total Duration by Region
```

```
@output =
SELECT
  Region,
  SUM(Duration) AS TotalDuration
FROM searchlog
GROUP BY Region;
```

This returns:

```
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

// find all the Regions where the total dwell time is &gt; 200

@output =

SELECT

Region,

SUM\(Duration\) AS TotalDuration

FROM @searchlog

GROUP BY Region

HAVING TotalDuration &gt; 200;

| en-fr | 241 |
| --- | --- |
| en-gb | 688 |
| en-gr | 305 |
| en-mx | 422 |
| en-us | 8291 |

// Count the number of total sessions.

@output =

SELECT

COUNT\(\) AS NumSessions

FROM @searchlog;

| 23 |
| --- |


Count the number of total sessions by Region.

@output =
SELECT
COUNT\(\) AS NumSessions,
Region
FROM @searchlog
GROUP BY Region;

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

@output =

SELECT
COUNT\(\) AS NumSessions,
Region,
SUM\(Duration\) AS TotalDuration,
AVG\(Duration\) AS AvgDwellTtime,
MAX\(Duration\) AS MaxDuration,
MIN\(Duration\) AS MinDuration
FROM @searchlog
GROUP BY Region;

| NumSessions:long | Region | TotalDuration:long | AvgDuration:double? | MaxDuration:int | MinDuration:int |
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

COUNT\(DISTINCT x\)

**DISTINCT** also works for user-defined aggregates.

MyAggregator\(DISTINCT x,y,z\)

Filtering on Aggregated Values

We’ll start again with a simply **GROUP BY**

@output =
SELECT
Region,
SUM\(Duration\) AS TotalDuration
FROM searchlog
GROUP BY Region;

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

SELECT  
  Region,  
  SUM\(Duration\) AS TotalDuration  
FROM @searchlog  
WHERE TotalDuration &gt; 200  
GROUP BY Region;  
\`\`\`

Which will cause an error. Because WHERE can only work on the input  
columns to the statement, not the output columns

We could of course, use multiple U-SQL Statements to accomplish this

@output =  
SELECT  
Region,  
SUM\(Duration\) AS TotalDuration  
FROM @searchlog  
GROUP BY Region;  
@output2 =  
SELECT \*  
FROM @output  
WHERE TotalDuration &gt; 200;

Alternatively , we can use the HAVING clause which is designed to filter  
columns when a GROUP BY is used..

@output =  
SELECT  
Region,  
SUM\(Duration\) AS TotalDuration  
FROM @searchlog  
GROUP BY Region  
HAVING SUM\(Duration\) &gt; 200;

You may have noticed that “SUM\(Duration\)” was repeated in the HAVING  
clause. That’s because HAVING \(like WHERE\) cannot use columns created in  
the SELECT clause.

If you try the code below, it will be a compilation error.

@output =  
SELECT  
Region,  
SUM\(Duration\) AS TotalDuration  
FROM @searchlog  
GROUP BY Region  
HAVING TotalDuration &gt; 200;

Breaking Rows Apart with CROSS APPLY

Let's examine the search log again.

@output =  
  SELECT  
    Region,  
    Urls  
  FROM @searchlog;

The query above returns something like this:

| Region | Urls |
| --- | --- |
| en-us | A;B;C |
| en-gb | D;E;F |

The **Urls** column contains strings, but each string is a  
semicolon-separated list of URLs. What happens if we want to break apart  
the **Urls** field so that only a URL is present on every row? For  
example, below is what we want to see:

| Region | Urls |
| --- | --- |
| en-us | A |
| en-us | B |
| en-us | C |
| en-gb | D |
| en-gb | E |
| en-gb | F |

This is a perfect job for the **CROSS APPLY** operator.

@output =  
  SELECT  
    Region,  
    Urls  
  FROM @searchlog;

@output =  
  SELECT  
    Region,  
    SqlArray.Create\(Urls.Split\(';'\)\) AS UrlTokens  
  FROM @output;

@output =  
  SELECT  
    Region,  
    Token AS Url  
FROM @output  
CROSS APPLY EXPLODE \(UrlTokens\) AS r\(Token\);

# Putting Rows Together with ARRAY\_AGG

The **LIST** aggregate operator performs the opposite of **CROSS  
APPLY**.

For example, if we start with this:

| Region | Result |
| --- | --- |
| en-us | A |
| en-us | B |
| en-us | C |
| en-gb | D |
| en-gb | E |
| en-gb | F |

But we want this as the output:

| Region | Urls |
| --- | --- |
| en-us | A;B;C |
| en-gb | D;E;F |

This is exactly what the **ARRAY\_AGG** operator does. In the example  
below you will see rowset taken apart by **CROSS APPLY** and then  
reconstructed via the **ARRAY\_AGG** operator.

@a = SELECT Region, Urls FROM @searchlog;  
@b =  
SELECT  
Region,  
SqlArray.Create\(Urls.Split\(';'\)\) AS UrlTokens  
FROM @a;

@c =  
SELECT  
  Region,  
  Token AS Url  
FROM @b  
CROSS APPLY EXPLODE \(UrlTokens\) AS r\(Token\);

@d =  
  SELECT Region,  
  string.Join\(";", ARRAY\_AGG&lt;string&gt;\(Url\).ToArray\(\)\) AS Urls  
  FROM @a

GROUP BY Region;

| Region | Urls |
| --- | --- |
| en-us | A;B;C |
| en-gb | D;E;F |

| Region | Urls |
| --- | --- |
| en-us | SqlArray&lt;string&gt;{“A”,”B”,”C”} |
| en-gb | SqlArray&lt;string&gt;{“D”,”E”,”F”} |

| Region | Result |
| --- | --- |
| en-us | A |
| en-us | B |
| en-us | C |
| en-gb | D |
| en-gb | E |
| en-gb | F |

| Region | Urls |
| --- | --- |
| en-us | A;B;C |
| en-gb | D;E;F |

# System-Defined Aggregates

U-SQL contains several common aggregation functions:

* AVG

* COUNT

* ANY\_VALUE

* FIRST\_VALUE

* LAST\_VALUE

* LIST

* MAX

* MIN

* SUM

* VAR \*

* STDEV \*

## ANY\_VALUE

**ANY\_VALUE** gets a value for that column with no implications about  
the where inside that rowset the value came from. It could be the first  
value, the last value, are on value in between. It is useful because in  
some scenarios \(for example when using Window Functions\) where you don't  
care which value you receive as long as you get one.

@output =

SELECT

ANY\_VALUE\(Start\) AS FirstStart,

Region

FROM @searchlog

GROUP BY Region;

## FIRST\_VALUE

---

As you can expect FIRST\_VALUE will return the first value. However, if  
you want to be specific about what qualifies as “First” use an ORDER BY  
clause. Otherwise, this aggregate will behave the same as ANY\_VALUE.

@output =

SELECT

FIRST\_VALUE\(Start\) AS FirstStart,

Region

FROM @searchlog

GROUP BY Region;

## LAST\_VALUE

As you can expect LAST\_VALUE will return the last value. However, if  
you want to be specific about what qualifies as “First” use an ORDER BY  
clause. Otherwise, this aggregate will behave the same as ANY\_VALUE.

@output =

SELECT

LAST\_VALUE\(Start\) AS FirstStart,

Region

FROM @searchlog

GROUP BY Region;

## Basic Statistics with MAX, MIN, AVG, STDEV, & SUM

These do what you expect them to do

@output =

SELECT

MAX\(Duration\) AS DurationMax,

MIN\(Duration\) AS DurationMin,

AVG\(Duration\) AS DurationAvg,

SUM\(Duration\) AS DurationSum,

VAR\(Duration\) AS DurationVarianve,

STDEV\(Duration\) AS DurationStDev,

FROM @searchlog

GROUP BY Region;

### For the Statisticians: An Important Fact about VAR and STDEV

**VAR** & **STDEV** are the **sample version** with Bessel's correction,  
**not** the better-known **population version**.

