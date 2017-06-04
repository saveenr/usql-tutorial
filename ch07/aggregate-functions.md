## System-Defined Aggregates

U-SQL contains several common aggregation functions:

* AVG
* COUNT
* ANY_VALUE
* FIRST_VALUE
* LAST_VALUE
* MAX
* MIN
* SUM
* VAR 
* STDEV
* ARRAY_AGG
* MAP_AGG 

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
```

## LAST_VALUE

As you can expect LAST_VALUE will return the last value. However, if  
you want to be specific about what qualifies as “First” use an ORDER BY  
clause. Otherwise, this aggregate will behave the same as ANY_VALUE.

```
@output =
  SELECT
    LAST_VALUE(Start) AS FirstStart,
    Region
  FROM @searchlog
  GROUP BY Region;
```

## Basic Statistics with MAX, MIN, AVG, STDEV, & SUM

These do what you expect them to do

```
@output =
  SELECT
    MAX(Duration) AS DurationMax,
    MIN(Duration) AS DurationMin,
    AVG(Duration) AS DurationAvg,
    SUM(Duration) AS DurationSum,
    VAR(Duration) AS DurationVariance,
    STDEV(Duration) AS DurationStDev,
  FROM @searchlog
  GROUP BY Region;
```

### Notes for Statisticians

**VAR** & **STDEV** are the **sample version** with Bessel's correction,**not** the better-known **population version**.
