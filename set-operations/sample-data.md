# Sample data

Let's define two RowSets: `@a` and `@b`. Notice that both RowSets have duplicate rows.

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





