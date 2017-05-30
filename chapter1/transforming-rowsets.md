# Transforming rowsets

We are going to start with the first script you encountered change it over-and-over to discover the basics of how U-SQL can be used to transform data.

Here is the starting script

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

OUTPUT @searchlog 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

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

RowSet Refinement

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



