## FIRST_VALUE

As you can expect FIRST_VALUE will return the first value. However, if
you want to be specific about what qualifies as “First” use an ORDER BY  
clause. Otherwise, this aggregate will behave the same as ANY_VALUE.
```

@output =
SELECT
FIRST_VALUE(Start) AS FirstStart,
Region
FROM @searchlog
GROUP BY Region;