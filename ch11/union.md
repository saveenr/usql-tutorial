# Combining two rowsets is done with the **UNION** operator.

**UNION ALL** will preserve any duplicates while **UNION DISTICT** will remove them.

```
@union\_distinct =   SELECT \* FROM @a

                        UNION DISTINCT

                    SELECT \* FROM @b;

@union\_all =        SELECT \* FROM @a

                        UNION ALL

                    SELECT \* FROM @b;
```

UNION DISTINCT

| DepID | Name |
| --- | --- |
| 1 | Smith |
| 2 | Brown |
| 3 | Case |
| 4 | Dey |


UNION ALL 

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


