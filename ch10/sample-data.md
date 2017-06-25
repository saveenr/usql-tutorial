# The input Data

To start, let&#39;s assume we have two rowsets as shown below:

```
@a  =
    SELECT * FROM
        (VALUES
            (1,    "Smith"),
            (1,    "Smith"),
            (2,    "Brown"),
            (3,    "Case")
        ) AS D( DepID, DepName );
```


```
@b  =
    SELECT * FROM
        (VALUES
            (1,    "Smith"),
            (1,    "Smith"),
            (1,    "Smith"),
            (2,    "Brown"),
            (4,    "Dey"),
            (4,    "Dey")
        ) AS D( DepID, DepName );
```





