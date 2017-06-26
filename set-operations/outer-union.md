#### U-SQL introduces `OUTER UNION` to enable easier handling of semistructured data

By default UNION requires both RowSets to have matching schemas. OUTER UNION allows the schemas to be different. If one RowSet is missing a column that the other has, that row will be included in the result with a default value for the missing columns.


NOTE: OUTER UNION only supports ALL. It does not support DISTINCT.

The following script will union the two rowsets `@left` and `@right` with the partially overlapping schema on columns `A` and `K` while filling in `null` into the "missing cells" of column `C` and `0` as the default value for type `int` for column `B`.

```
@left =
    SELECT *
    FROM (VALUES ( 1, "x", (int?) 50 ),
                 ( 1, "y", (int?) 60 )
         ) AS L(K, A, C);

@right =
    SELECT *
    FROM (VALUES ( 5, "x", 1 ),
                 ( 6, "x", 2 ),
                 (10, "y", 3 )
         ) AS R(B, A, K);

@res =
    SELECT * FROM @left
    OUTER UNION BY NAME ON (*)
    SELECT * FROM @right;
```

The result is:

K | A   | C  | B  |
- | --- | -- | -- |
1 | "x" | 50 |  0 |
1 | "x" |    |  5 |
1 | "y" | 60 |  0 |
2 | "x"	|    |  6 |
3 | "y" |    | 10 |


