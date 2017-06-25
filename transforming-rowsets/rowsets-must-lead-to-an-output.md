## All RowSets must lead to an output

There's no syntax error here. However, compilation will fail. The reason is that all rowsets must eventually contribute to data being written to a file or table. And so the following script fail will to compile the `@smallrows` does not eventually result in an output to a file or table

```
@rows =
 EXTRACT
   Name string,
   Amount int,
   FROM "/input.csv"
 USING Extractors.Csv();

// THIS WILL CAUSE A SYNTAX ERROR
@smallrows =
  SELECT Name, Amount
  FROM @rows
  WHERE Amount < 1000;

OUTPUT @rows
  TO "/output/output.csv"
  USING Outputters.Csv();
```

