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

## FileSets

We's seen that you can explicitly list all the files in the EXTRACT statement. You may not want to list all the files manually because there are a lot them.

FileSets make it easy to define a pattern to identify a set of files to read. 

In the simplest case let's get all the files in a folder.

```
@rs =
    EXTRACT 
        user   string,
        id     string,
    FROM 
        "/input/{*}"
    USING Extractors.Csv();
```

### Getting information about the filenames into the RowSet

Because we are reading rows from multiple files. it is convenient to for the rows to have some information about the filename it came from. We can adjust the query slightly to make this possible.

```
@rs =
    EXTRACT 
        user       string,
        id         string,
        __filename string
    FROM 
        "/input/{__filename}"
    USING Extractors.Csv();
```

You are probably wondering about the `__` in the column `__filename`. It isn't necessary at all, however it is useful as a way of marking that this information came from the process of extracting the file, not from the data in the file itself.

### Specifying a list of files that have an extension

This is very simple modification syntax. The example uses will extract all the files that end with ".csv".

```
@rs =
    EXTRACT 
        user   string,
        id     string,
    FROM 
        "/input/{*}.csv"
    USING Extractors.Csv();
```

## Using WHERE to filter the files.

Suppose you have a folder with 5000 files that are names as "data1".csv, "data2.csv", ..., "data4999.csv", "data5000.csv". Often you want the only a certain range of those files for example only that match  7 to 21.

First, lets see how to match the basic pattern of "data + num + .csv"

```
@rs =
    EXTRACT 
        user       string,
        id         string
    FROM 
        "/input/data{*}.csv"
    USING Extractors.Csv();
```

Now to get the file number into the RowSet.

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

Right now the script will still attempt to read all 5000 files. But because we have added the \_\_filenum column from the extraction creating future rowsets can use that information to avoid reading uneccessary files. 

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
        ( __filenum >= 7 ) 
        AND (__filenum <= 21);
```





