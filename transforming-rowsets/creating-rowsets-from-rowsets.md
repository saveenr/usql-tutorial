# Creating a RowSet from another RowSet

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
    FROM @searchlog;

OUTPUT @output 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

Effectively this script just copies the data without transforming it.

**However**, if you look at the output file you will notice some things about the default behavior of `Outputters.Tsv`:

* values have been surrounded by double-quotes - which is the default behavior of `Outputters.Tsv`. You can disable the quoting by using `Outputters.Tsv( quoting:false )`.
* `DateTime` values are output in a longer standard format
* The default encoding is UTF-8 and the BOM \(Byte Order Mark\) is not written.



