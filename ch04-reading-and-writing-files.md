

# Header Rows in Text Files



```
@rows =
  EXTRACT
    Name string,
    Id int,
 FROM "/input.csv"
 USING Extractors.Csv(skipFirstNRows:1);

OUTPUT @rows
  TO "/output/output.csv"
  USING Outputters.Csv(outputHeader:true);
```

# Multiple inputs for EXTRACT

```
@rows =
  EXTRACT name string, idint
  FROM
    "/file1.tsv",
    "/file2.tsv",
    "/file3.tsv"
  USING Extractors.Csv( );
```



