# Combining two rowsets is done with the **UNION** operator.

**UNION ALL** will preserve any duplicates while **UNION DISTICT** will remove them.

```
@union_distinct = 
    SELECT * FROM @a
    UNION DISTINCT
    SELECT * FROM @b;

@union_all = 
    SELECT * FROM @a
    UNION ALL
    SELECT * FROM @b;
```

@union_distinct

| DepID | Name |
| --- | --- |
| 1 | Smith |
| 2 | Brown |
| 3 | Case |
| 4 | Dey |


@union_all

| DepID | Name |
| --- | --- |
| 1 | Smith |
| 1 | Smith |
| 2 | Brown |
| 3 | Case |
| 1 | Smith |
| 1 | Smith |
| 1 | Smith |
| 2 | Brown |
| 4 | Dey |
| 4 | Dey |


