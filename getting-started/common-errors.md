# Common compilation errors

When something is wrong with a U-SQL script it will not compile. Fortunately, of the many causes for compilation failure, there are only a few of them that account for the vast majority of compilation failures.

It will save you time to be familiar with these issues and how they manifest themselves in error messages and in the tools.

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

## Input file does not exist

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





