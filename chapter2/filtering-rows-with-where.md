## Filtering records with WHERE

Now let's transform the data by filtering out records with the WHERE clause

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

OUTPUT @output 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

## 



