## Reusing U-SQL code with a table-valued function \(TVF\)

Many of the scripts in the tutorial have required reading from the searchlog and the code to read from the searchlog is shown below

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
```

Now instead of writing this code over and over in every script we will store the code as a TVF in a database. The name of this function is going to be `MyDB.dbo.ExtractSearchLog`. The `dbo` part of that name is the "schema".

```
CREATE FUNCTION MyDB.dbo.ExtractSearchLog()
RETURNS @rows 
AS BEGIN
  @rows = 
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    FROM "/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
END;
```

Notice that CREATE FUNCTION indicates it will return a RowSet call @rows. Then in the @rows RowSet is defined.

Now that the TVF is created, we can call it this way.

```
@searchlog = MyDB.dbo.ExtractSearchLog();
```

