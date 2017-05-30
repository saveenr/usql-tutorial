

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





