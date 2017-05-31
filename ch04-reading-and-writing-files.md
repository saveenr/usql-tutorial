## Skipping Header Rows in Text Files

With `Extractors.Csv` and `Extractors.Tsv` you can use the `skipFirstNRows` parameter to skip a number of lines at the beginning on a file.

```
@rows =
  EXTRACT
    Name string,
    Id int,
 FROM "/input.csv"
 USING Extractors.Csv(skipFirstNRows:1);
```

## Writing header row in text files

The `Outputters.Csv` and `Outputters.Tsv` statement has a parameter called outputHeader that controls whether outfile files will begin with a header row.

```
OUTPUT @rows
  TO "/output/output.csv"
  USING Outputters.Csv(outputHeader:true);
```

## Multiple inputs for EXTRACT

EXTRACT works with a comma-separated list of file paths. Keep in mind that the files extracted this way must have matching schemas.

```
@rows =
  EXTRACT 
    Name string, 
    Id int
  FROM
    "/file1.tsv",
    "/file2.tsv",
    "/file3.tsv"
  USING Extractors.Csv( );
```



