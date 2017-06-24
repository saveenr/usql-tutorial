


# Finding Common Rows with INTERSECT

Sometimes, we only care about the rows both rowsets have _in common_. We use the **INTERSECT** operator to accomplish this. **INTERSECT ALL** preserves duplicates while **INTERSECT** removes duplicates.

## INTERSECT ALL

```
@intersect_all =
    SELECT \* FROM @a
    INTERSECT ALL
    SELECT \* FROM @b;
```

@intersect_all

| DepID | name |
| --- | --- |
| 1 | Smith |
| 1 | Smith |
| 2 | Brown |

## INTERSECT DISTINCT

```
@intersect_distinct =
    SELECT * FROM @a
    INTERSECT DISTINCT
    SELECT * FROM @b;
```

@intersect_distinct

| DepID | name |
| --- | --- |
| 1 | Smith |
| 2 | Brown |

