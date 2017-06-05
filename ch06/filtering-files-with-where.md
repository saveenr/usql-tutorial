## Using WHERE to filter the files

FileSets also let us filter the inputs so that the EXTRACT only chooses some of those files. For example all the files between "data7.csv" and "data21.csv". This is done by using WHERE with the columns coming from the FileSet.

```
@rs =
    EXTRACT 
        user       string,
        id         string,
        __filenum  int,
    FROM 
        "/input/data{__filenum}.csv"
    USING Extractors.Csv();

@rs =
    SELECT *
    FROM @rs
    WHERE 
        ( __filenum >= 7  ) AND 
        ( __filenum <= 21 );
```

## 



