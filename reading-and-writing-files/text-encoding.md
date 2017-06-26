# Text Encoding

By default all the extractors default to UTF-8 as the encoding.

All three built-in extractors allow you to control the encoding through the **encoding** parameter as shown below.

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
    USING Extractors.Tsv( encoding: Encoding.[ASCII] );
```

These are the supported text encodings:

```
Encoding.[ASCII]
Encoding.BigEndianUnicode
Encoding.Unicode
Encoding.UTF7
Encoding.UTF8
Encoding.UTF32
```
