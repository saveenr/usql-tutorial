## FileSets with dates

File names or paths often include information about dates and times. This is often the case for log files. FileSets make it easy to handle these cases.

Image we have files that are named in this pattern `"data-YEAR-MONTH-DAY.csv"`. The following query reads all the files where with that pattern.

```
@rs = 
  EXTRACT 
      user    string,
      id      string,
      __date  DateTime
  FROM 
    "/input/data-{__date:yyyy}-{__date:MM}-{__date:dd}.csv"
  USING Extractors.Csv();
```

Many times you'll have to restrict the files to a specific time range. This can be done by refining the RowSet with a WHERE clause.

```
@rs = 
  EXTRACT 
      user    string,
      id      string,
      __date  DateTime
  FROM 
    "/input/data-{__date:yyyy}-{__date:MM}-{__date:dd}.csv"
  USING Extractors.Csv();

@rs = 
  SELECT * 
  FROM @rs
  WHERE 
    date >= System.DateTime.Parse("2016/1/1") AND
    date < System.DateTime.Parse("2016/2/1");
```

In the above example we used all three parts of the date in the file path. However, you can use any parts you need and don't need to use them all. The following example shows a file path that only uses the year and month.

```
@rs =
    EXTRACT 
      user    string,
      id      string,
      __date  DateTime
  FROM 
    "/input/data-{__date:yyyy}-{__date:MM}.csv"
  USING Extractors.Csv();

@rs = 
  SELECT * 
  FROM @rs
  WHERE
    date >= System.DateTime.Parse("2016/1/1") AND
    date < System.DateTime.Parse("2016/2/1");
```



