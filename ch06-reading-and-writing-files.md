# Reading and Writing Files

## File paths with EXTRACT and OUTPUT

EXTRACT and OUTPUT naturally have to identify the paths to the files they read or write. U-SQL supports three forms of file paths.

**Relative paths**

Example:

`"/somefile.txt"`

Paths that begin with "/" is interpreted as:

* Relative the the root of the Local Data Root **during U-SQL Local Execution**
* Relative the the root of the default store of the ADLA account **during U-SQL Cloud Execution** 

**Absolute Data Lake Store paths**

`"adl://myadlsaccount.azuredatalakestore.net/somefile.txt"`

**Absolute Windows Blob storage paths**

`"wasb://container@storageaccount.blob.core.windows.net/somefile.txt"`

## Built-in Extractors

U-SQL has built-in Extractors to deal with CSV and TSV file.

* `Extractors.Csv` - for comma-separated-value files
* `Extractors.Tsv` - for tab-separated-value files

## Built-in Outputters

U-SQL has built-in Extractors to deal with CSV and TSV file.

* `Outputters.Csv` - for comma-separated-value files
* `Outputters.Tsv` - for tab-separated-value files

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
    "/file1.csv",
    "/file2.csv",
    "/file3.csv"
  USING Extractors.Csv( );
```

## Getting the first N rows of a file


```
@a =
  EXTRACT 
    Name string, 
    Id int
  FROM
    "/file1.csv"
  USING Extractors.Csv( );

@a = 
  SELECT * 
  FROM @a
  ORDER BY 1
  FETCH FIRST 100 ROWS;	
```

This syntax may look misleading. The `FETCH FIRST` part should be obvious. The `ORDER BY 1 ASC` is the part we need to understand. You should read this as "order the rows on the constant value 1'. It DOES NOT mean "order the rows on the first column". 

We are going to cover `ORDER BY` in much greater detail on other chapters.



