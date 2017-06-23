


# Finding Common Rows with INTERSECT

Sometimes, we only care about the rows both rowsets have _in common_. We use the **INTERSECT** operator to accomplish this. **INTERSECT ALL** preserves duplicates while **INTERSECT** removes duplicates.



@intersect\_distinct =

    SELECT \* FROM @a

    INTERSECT DISTINCT

    SELECT \* FROM @b;

@intersect\_all =

    SELECT \* FROM @a

    INTERSECT ALL

    SELECT \* FROM @b;

OUTPUT @intersect\_distinct

    TO @&quot;/Samples/Output/intersect\_distinct.txt&quot;

    USING Outputters.Tsv();

OUTPUT @intersect\_all

    TO @&quot;/Samples/Output/intersect\_all.txt&quot;

    USING Outputters.Tsv();

| INTERSECT ALL  |
| --- |
|

| DepID | name |
| --- | --- |
| 1 | Smith |
| 1 | Smith |
| 2 | Brown |

   |
| INTERSECT DISTINCT  |
|

| DepID | name |
| --- | --- |
| 1 | Smith |
| 2 | Brown |

  |

