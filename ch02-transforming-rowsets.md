## Creating a new rowset from another rowset

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
    FROM @searchlog;

OUTPUT @output 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

This script doesn't change the output of the script at all. But it does introduce the basic mechanism of how a rowset is defined from another rowset.


## Creating constant rowsets



@departments =

 SELECT \* FROM

 \(VALUES

 \(31, "Sales"\),

 \(33, "Engineering"\),

 \(34, "Clerical"\),

 \(35, "Marketing"\)

 \) AS

 D\(DepID,DepName\);





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

## Adding new columns with SELECT

We can create new Columns with the SELECT clause. Simply use a C\# expression and give it a column name with AS.

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
        *, 
        Query+Query AS Query2
    FROM @searchlog;

OUTPUT @output 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

In the example above, we added a column without having to explicitly list out all the other columns. Below you can see how we can add just specific columns.

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
        Query+Query AS Query2, 
        Urls.Length AS UrlsLength
    FROM @searchlog;

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



# Find all the sessions occurring between two dates

```
@output =
 SELECT Start, Region, Duration
 FROM @searchlog
 WHERE Start <= DateTime.Parse("2012/02/17");
```





// Find all the sessions occurring between two dates

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




// SYNTAX ERROR

@output =

 SELECT Start, Region,Duration/60.0 ASDurationInMinutes

 FROM @searchlog

 WHEREDurationInMinutes&gt;= 20;



// SYNTAX ERROR

output =

 SELECT Start, Region,Duration/60.0 ASDurationInMinutes

 FROM @searchlog

HAVINGDurationInMinutes&gt;= 20;







// THIS WORKS: Refine the rowset



@output =

 SELECT Start,

 Region,

Duration/60.0 ASDurationInMinutes

 FROM @searchlog;



@output =

 SELECT \*

 FROM @output

 WHEREDurationInMinutes&gt;= 20;





// THIS WORKS: Repeat the full expression



@output =

 SELECT Start,

 Region,

Duration/60.0 ASDurationInMinutes

 FROM @searchlog

 WHEREDuration/60.0&gt;= 20;




## Numbering Rows

Using the **ROW\_NUMBER** windowing function aggregate is how to assign row numbers. ROW\_NUMBER is part of Windowing Functions and that topic too complex for this tutorial – See the Windowing Functions documentation for details. However, for now we do want to show you the proper way to number rows in U-SQL using ROW\_NUMBER because it is a popular topic.

```
@rs1 =
 SELECT
   ROW_NUMBER() OVER ( ) AS RowNumber,
   Start,
   Region
 FROM @searchlog;
```


























