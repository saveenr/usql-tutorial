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

## The SQL Logical Operators

The **AND**/**OR**/**NOT** operators can be combined with parentheses to create more complex logical expressions

```
@output =
    SELECT Start, Region, Duration
    FROM @searchlog
    WHERE (Duration >= 2*60 AND Duration <= 5*60) OR NOT (Region == "en-gb");
```

## The C# Logical Operators

U-SQL also supports the C\# logical operators

```
@output =
 SELECT Start, Region, Duration
 FROM @searchlog
 WHERE (Duration >= 2*60 && Duration <= 5*60) || (!(Region == "en-gb"));
```

## SQL Logical Operators (AND OR) versus C\# Operators ( && || )

These operators behave the same except for their short-circuiting behavior:

* SQL-style logical operators: These DO NOT short-circuit
* C#-style logical operators: These DO short-circuit

Use the SQL-style logical operators unless you MUST have the short-circuiting behavior. The reasons why this is important are covered in a later chapter that describes the order of evaluation of predicates in expressions.

## Find all the sessions occurring before a date

```
@output =
 SELECT Start, Region, Duration
 FROM @searchlog
 WHERE Start <= DateTime.Parse("2012/02/17");
```

## Find all the sessions occurring between two dates

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
  SELECT 
      Start, 
      Region,
      Duration/60.0 AS DurationInMinutes
  FROM @searchlog;
```

There are a couple of approaches for filtering rows based on the `DurationInMinutes` value

#### The first option is to use RowSet refinement

```
@output =
  SELECT 
    Start,
    Region,
    Duration/60.0 ASDurationInMinutes
  FROM @searchlog;

@output =
  SELECT *
  FROM @output
  WHERE DurationInMinutes>= 20;
```

#### The second option is to repeat the expression in the WHERE clause

```
@output =
  SELECT 
    Start,
    Region,
    Duration/60.0 AS DurationInMinutes
  FROM @searchlog
    WHERE Duration/60.0>= 20;
```

#### WHERE does not work on calculated columns in the same statement

`WHERE` filters rows coming into to the statement. The DurationInMinutes column doesn't exist in the input. Therefore WHERE cannot operate on it. So, the example below will not compile

```
// SYNTAX ERROR: WHERE cannot be used with columns created by SELECT
@output =
  SELECT Start, Region,Duration/60.0 AS DurationInMinutes
  FROM @searchlog
  WHERE DurationInMinutes>= 20;
```



