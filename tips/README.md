# Tips

### Generating ranges of numbers and dates

Many common scenarios for U-SQL developers require constructing a RowSet made up of a simple range of numbers or dates, for example the integers from 1 to 10. In this blog post we'll take a look at options for doing this in U-SQL. In the process, we'll get a chance to learn how to use some common U-SQL features:

* Creating RowSets from constant values
* Using CROSS JOIN
* Using SELECT to map integers to DateTimes
* Using CREATE TABLE to create a table directly from a RowSet. This is sometimes called "CREATE TABLE AS SELECT" and often abbreviated as "CTAS".

First, we'll begin by using the VALUES statement to create a simple RowSet of integers from 0 to

```
@numbers_10 = 
    SELECT*
    FROM (VALUES
          (0),
          (1),
          (2),
          (3),
          (4),
          (5),
          (6),
          (7),
          (8),
          (9)
    ) AS T(Value);
```

This technique is simple. However, it's disadvantages are: \(1\) the script will need to manually list all the numbers \(2\) there is an upper limit on the number of items allowed in VALUES in a U-SQL script. Currently that limit is 10,000 items.

The CROSS JOIN statement helps us generate larger lists of numbers easily. In the following snippet, CROSS JOIN is used to generate 100 integers from 0 to 99.

```
@numbers_100 = 
    SELECT (a.Value*10 + b.Value) AS Value
    FROM @numbers_10 AS a 
        CROSS JOIN @numbers_10 AS b;
```

We can apply CROSS JOIN again to increase this to generate 10,000 integers from 0 to 9,999 as shown in the following sample:

```
@numbers_10000 = 
    SELECT (a.Value*100 + b.Value) AS Value
    FROM @numbers_100 AS a CROSS JOIN @numbers_100 AS b;
```

Finally, if you want a range of dates, use SELECT to map each integer to a DateTime value as shown below.

```
DECLARE @StartDate = DateTime.Parse("1979-03-31");

@numbers_10000 = ...;

@result = 
    SELECT 
        Value,
        @StartDate.AddDays( Value ) AS Date
    FROM @numbers_10000;
```

Putting it all together, the full script looks like this:

```
DECLARE @StartDate = DateTime.Parse("1979-03-31");

@numbers_10 = 
    SELECT
        *
    FROM 
    (VALUES
        (0),
        (1),
        (2),
        (3),
        (4),
        (5),
        (6),
        (7),
        (8),
        (9)
    ) AS T(Value);

@numbers_100 = 
    SELECT (a.Value*10 + b.Value) AS Value
    FROM @numbers_10 AS a CROSS JOIN @numbers_10 AS b;

@numbers_10000 = 
    SELECT (a.Value*100 + b.Value) AS Value
    FROM @numbers_100 AS a CROSS JOIN @numbers_100 AS b;

@result = 
    SELECT 
        Value,
        @StartDate.AddDays( Value ) AS Date
    FROM @numbers_10000;

OUTPUT @result TO "/res.csv" USING Outputters.Csv(outputHeader:true);
```

Ultimately, it may be more convenient to store these ranges in a U-SQL Table rather than regenerating them every time in a script. This is easy with CTAS. The snippet below shows how to create a table using CREATE TABLE from a RowSet that contains these numbers. Notice that in this case CREATE TABLE does not require the schema to be specified. The scheme is inferred from the SELECT clause.

```
CREATE DATABASE IF NOT EXISTS MyDB;
DROP TABLE IF EXISTS MyDB.dbo.Numbers_10000;

CREATE TABLE MyDB.dbo.Numbers_10000
( 
 INDEX idx 
 CLUSTERED(Value ASC)
 DISTRIBUTED BY RANGE(Value) 
) AS SELECT * FROM @numbers_10000
```

Now retrieving any desired range is simple by using SELECT on the table followed with a WHERE clause:

```
// get the range of numbers from 1 to 87
@a = 
    SELECT Value 
    FROM MyDB.dbo.Numbers_10000
    WHERE Value >=1 AND Value <= 87;
```



