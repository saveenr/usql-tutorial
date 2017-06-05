# Common errors

Of the many ways in which a U-SQL script is not compilable, there are only a few that account for the vast majority of issues. It WILL save you time to be familiar with the issues and how they manifest themselves in error messages and in the tools.

Try running each of the sample scripts below.

## Incorrect casing on U-SQL identifiers

The script below uses `from` instead of `FROM`.

```
@searchlog = 
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    from @"/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog 
    TO @"/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

## Input does not exist

```
@searchlog = 
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    FROM "/SearchLog_does_not_exist.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

## Invalid C\# Expression

```
@searchlog = 
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    FROM "/SearchLog.tsv"
    USING Extractors.Tsv_xyz();

OUTPUT @searchlog 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

Take a look at the TSV file. Some things to point out

* there is no header row
* it is a tsv - tab-separated-values



