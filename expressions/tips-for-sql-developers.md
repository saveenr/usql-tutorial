
## A Tip for SQL developers

Lots of people come to U-SQL from SQL and ask how U-SQL accomplishes things they are familiar with in SQL. A great example in creating an uppercase string.

Given, what we' ve covered so far A SQL developer will expect to write the following in U-SQL

```
@output = 
    SELECT 
        UPPER( Region ) AS NewRegion
    FROM @searchlog;
```
Unfortunately, said developer will be disappointed to find out that U-SQL has no `UPPER()` method. The C# developer, on the other hand, knows what to do: use the string type's intrinsic `ToUpper()` method.

```
@output = 
    SELECT 
        Region.ToUpper() AS NewRegion
    FROM @searchlog;
```

