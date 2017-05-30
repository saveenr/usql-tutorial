## Adding new columns with SELECT

We can create new Columns with the SELECT clause. Simply use a C\# expression and give it a column name with AS.

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
        *, 
        Query+Query AS Query2
    FROM @searchlog;

OUTPUT @output 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

In the example above, we added a column without having to explicitly list out all the other columns. Below you can see how we can add just specific columns.

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
        Query+Query AS Query2, 
        Urls.Length AS UrlsLength
    FROM @searchlog;

OUTPUT @output 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```



