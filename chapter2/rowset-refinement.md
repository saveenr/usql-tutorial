## RowSet Refinement

Rowsets can be defined from other rowsets. In fact rowsets can be defined from themselves. This is called RowSet refinement and can be a powerful way to make your code easier to read.

Consider the example below @output has columns defined with the SELECT clause and the rows are filtered with the WHERE clause.

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
    USING Extractors.Tsv();

@output = 
    SELECT 
        Region,
        Query, 
        Urls.Length AS UrlsLength
    FROM @searchlog
    WHERE Region == "en-gb";

OUTPUT @output 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

It can be done in two steps in the script

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
    USING Extractors.Tsv();

@output = 
    SELECT *
    FROM @searchlog
    WHERE Region == "en-gb";

@output = 
    SELECT 
        Region,
        Query, 
        Urls.Length AS UrlsLength
    FROM @output;

OUTPUT @output 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```



