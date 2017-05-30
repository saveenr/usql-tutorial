

// Find all the sessions occurring between two dates

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



