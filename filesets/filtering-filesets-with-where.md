# Using WHERE to filter the files

FileSets also let us filter the inputs so that the EXTRACT only chooses some of those files. 


For example, imagine we have 10 files as shown below.

```
/input/data1.csv
/input/data2.csv
/input/data3.csv
/input/data4.csv
/input/data5.csv
/input/data6.csv
/input/data7.csv
/input/data8.csv
/input/data9.csv
/input/data10.csv
```

The following EXTRACT will read all 10 files

```
@rs =
    EXTRACT 
        user       string,
        id         string,
        __filenum  int,
    FROM 
        "/input/data{__filenum}.csv"
    USING Extractors.Csv();
```

However by adding a WHERE clause that uses `__filenum` only those files that are matched by the WHERE clause are read. Only 3 files are read (7,8,9).

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
        ( __filenum <= 9 );
```

