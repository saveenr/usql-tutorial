# UNION

**UNION** combines two rowsets.

## UNION ALL

As you can see UNION ALL clearly leaves in duplicate rows.

```
@union_all = 
    SELECT * FROM @a
    UNION ALL
    SELECT * FROM @b;
```

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


## UNION DISTINCT

UNIONS DISTINCT discards duplicate rows.

```
@union_distinct =
    SELECT * FROM @a
    UNION DISTINCT
    SELECT * FROM @b;
```

| DepID | Name |
| --- | --- |
| 1 | Smith |
| 2 | Brown |
| 3 | Case |
| 4 | Dey |

## Schema requirements

UNION by default require that the RowSets have the same schema.

* Each column must have the same name and data type
* The columns must appear in the same order in the rowset schema

