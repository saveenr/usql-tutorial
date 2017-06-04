## Filtering records with WHERE

Now let's transform the data by filtering out records with the WHERE clause

```
@searchlog = 
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    FROM "/SearchLog.tsv"
    USING Extractors.Tsv();

@output = 
    SELECT *
    FROM @searchlog
    WHERE Region == "en-gb";

OUTPUT @output 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```


## RowSet Refinement

Rowsets can be defined from other rowsets. In fact rowsets can be defined from themselves. This is called RowSet refinement and can be a powerful way to make your code easier to read.

Consider the example below @output has columns defined with the SELECT clause and the rows are filtered with the WHERE clause.

```
@searchlog = 
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    FROM "/SearchLog.tsv"
    USING Extractors.Tsv();

@output = 
    SELECT 
        Region,
        Query, 
        Urls.Length AS UrlsLength
    FROM @searchlog
    WHERE Region == "en-gb";

OUTPUT @output 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

It can be done in two steps in the script

```
@searchlog = 
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    FROM "/SearchLog.tsv"
    USING Extractors.Tsv();

@output = 
    SELECT *
    FROM @searchlog
    WHERE Region == "en-gb";

@output = 
    SELECT 
        Region,
        Query, 
        Urls.Length AS UrlsLength
    FROM @output;

OUTPUT @output 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

## The SQL Logical Operators

The **AND**/**OR**/**NOT** operators can be combined with parentheses for more complex expressions

```
@output =
 SELECT Start, Region, Duration
 FROM @searchlog
 WHERE (Duration >= 2*60 AND Duration <= 5*60) OR NOT (Region == "en-gb");
```

## The C\# Logical Operators

U-SQL also supports the C\# logical operators

```
@output =
 SELECT Start, Region, Duration
 FROM @searchlog
 WHERE (Duration >= 2*60 && Duration <= 5*60) || (!(Region == "en-gb"));
```

## SQL Logical Operators versus C\# Operators

Answer: Use the SQL-style logical unless you have to use the C\# versions.

That answer clearly suggests there is a difference - and it is an important one. A part of standard U-SQL optimization predicates in the WHERE clause may be reordered when the script is executed. Specifically, they DO NOT perform short-circuiting behavior. The C\# logical operators DO provide short circuiting.

Find all the sessions occurring between two dates

```
@output =
 SELECT Start, Region, Duration
 FROM @searchlog
 WHERE Start <= DateTime.Parse("2012/02/17");
```

Find all the sessions occurring between two dates

```
@output = 
  SELECT Start, Region, Duration
  FROM @searchlog
  WHERE 
   Start >= DateTime.Parse("2012/02/16") 
   AND Start <= DateTime.Parse("2012/02/17");
```

## Testing for Membership with the IN Operator

You can use the **IN** operator as shown below to test for membership in a set of values.

```
@rs =
 SELECT FirstName, LastName, JobTitle
 FROM People
 WHERE JobTitle IN ("Design Engineer", "Tool Designer", "Marketing Assistant");
```

Keep in mind that the U-SQL **IN** operator does not offer all the features of the SQL **IN** operator.

## Filtering on calculated columns

Consider this case where SELECT is used to define a new column called `DurationInMinutes`

```
@output =
  SELECT Start, Region,Duration/60.0 AS DurationInMinutes
  FROM @searchlog;
```

There are a couple of approaches for filtering rows based on the `DurationInMinutes` value

The first option is to use RowSet refinement

```
@output =
  SELECT Start,
    Region,
    Duration/60.0 ASDurationInMinutes
  FROM @searchlog;

@output =
  SELECT *
  FROM @output
  WHERE DurationInMinutes>= 20;
```

The second option is to repeat the expression in the WHERE clause

```
@output =
  SELECT 
    Start,
    Region,
    Duration/60.0 AS DurationInMinutes
  FROM @searchlog
    WHERE Duration/60.0>= 20;
```

However, you might wonder why the expression is repeated. You might ask why not simply use `DurationInMinutes` in the `WHERE` clause below the `SELECT` that defined it. The short answer is that will not compile.

```
// SYNTAX ERROR
@output =
  SELECT Start, Region,Duration/60.0 AS DurationInMinutes
  FROM @searchlog
  WHERE DurationInMinutes>= 20;
```

Let's understand why. `WHERE` filters rows coming to the statement. The DurationInMinutes column doesn't exist in the input. Therefore WHERE cannot operate on it.

You might be tempted to use the `HAVING` clause \(which we haven't covered yet in this tutorial\). But although `HAVING` does filter outgoing rows from a statement, it also requires a GROUP BY clause. So it can't help in this scenario.

```
// SYNTAX ERROR
@output =
  SELECT Start, Region,Duration/60.0 AS DurationInMinutes
  FROM @searchlog
  HAVING DurationInMinutes>= 20;
```

## Numbering rows

Using the `ROW_NUMBER` windowing function aggregate is how to assign row numbers. `ROW_NUMBER` is part of Windowing Functions and that topic too complex for this tutorial.  See the Windowing Functions documentation for details. However, for now we do want to show you the proper way to number rows in U-SQL using `ROW_NUMBER` because it is a popular topic.

```
@rs1 =
 SELECT
   ROW_NUMBER() OVER ( ) AS RowNumber,
   Start,
   Region
 FROM @searchlog;
```

There's no syntax error here. But compilation will fail. The reason is that all rowsets must eventually contribute to data being written to a file or table. And so the following script fail will to compile the `@smallrows`does not eventually result in an output to a file or table

```
@rows =
 EXTRACT
   Name string,
   Amountint,
   FROM "/input.csv"
 USING Extractors.Csv();

// THIS WILL CAUSE A SYNTAX ERROR
@smallrows =
  SELECT Name, Amount
  FROM @rows
  WHERE Amount < 1000;

OUTPUT @rows
  TO "/output/output.csv"
  USING Outputters.Csv();
```

# Escaping Column names

```
@b =
SELECT[Order Number], Part
FROM @a;
```



