# U-SQL Expressions

## Overview

Clauses such as `SELECT`, `WHERE`, and `HAVING` (among others) allow you to enter U-SQL expressions.

An expression in a programming language is a combination of explicit values, constants, variables, operators, and functions that are interpreted according to the particular rules of precedence and of association for a particular programming language, which computes and then produces another value.

The simplest way of thinking of a U-SQL expression is that it is a merely C# expression with some U-SQL extensions such as the `AND`, `OR`, `NOT` operators.

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

RowSet columns are strongly typed. U-SQL allows you to call methods defined on those types in the SELECT clause.  

```
// Find what day of year each session took place

@output= 
    SELECT 
        Start, 
        Region, 
        Start.DayOfYear AS StartDayOfYear
    FROM @searchlog;
```
