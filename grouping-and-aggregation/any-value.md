## ANY_VALUE

**ANY_VALUE** gets a value for that column with no implications about the where inside that rowset the value came from. It could be the first value, the last value, are on value in between. It is useful because in some scenarios (for example when using Window Functions) where you don't care which value you receive as long as you get one.

```
@output =
  SELECT
    ANY_VALUE(Start) AS FirstStart,
    Region
  FROM @searchlog
  GROUP BY Region;
```

