# U-SQL Expressions

## Overview

Clauses such as SELECT, WHERE, and HAVING \(and others\) allow you to enter expressions - in particular U-SQL Expressions.

An expression in a programming language is a combination of explicit values, constants, variables, operators, and functions that are interpreted according to the particular rules of precedence and of association for a particular programming language, which computes and then produces another value.

The simplest way of thinking of a U-SQL expression is that it is a merely C\# expression with some U-SQL extensions such as the AND, OR, NOT operators.

## A Tip for SQL Developers

Lots of people come to U-SQL from SQL and ask how U-SQL accomplishes things they are familiar with in SQL. A great example in creating an uppercase string.

Given, what we' ve covered so far A SQL developer will expect to write the following in U-SQL

```
@output = 
    SELECT 
        UPPER( Region ) AS NewRegion
    FROM @searchlog;
```

But those developers will be disappointed to find out that U-SQL has no UPPER\(\) method. The C\# developer knows what to: just use the string type's intrinsic ToUpper\(\) method.

```
@output = 
    SELECT 
        Region.ToUpper() AS NewRegion
    FROM @searchlog;
```

### Casting types

Expressions can also be converted to a different type

```
@output= 
    SELECT 
        Start, 
        Region, 
        ((double) Duration) AS DurationDouble
    FROM @searchlog;
```

### Using .NET Types

Rowset columns are strongly typed. U-SQL allows you to call methods defined on those types in the SELECT clause.  
// Find what day of year each session took place

```
@output= 
    SELECT 
        Start, 
        Region, 
        Start.DayOfYear AS StartDayOfYear
    FROM @searchlog;
```

### 



