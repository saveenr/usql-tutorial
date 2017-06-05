## RowSet refinement

Rowsets can be defined from other rowsets. In fact rowsets can be defined from themselves. This is called RowSet refinement and can be a powerful way to make your code easier to read.


```
@output = 
    SELECT
        *,
        Urls.Length AS UrlsLength
FROM @searchlog;


@output = 
    SELECT 
        Url,
        UrlLength
    FROM @output1;

OUTPUT @output 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```



