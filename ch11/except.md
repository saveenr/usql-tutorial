
# Finding Rows That Are NOT in the Other RowSet with EXCEPT

The **EXCEPT** operator returns all the rows in the left RowSet that _are not_ in the right RowSet.



```
@a_except_b_all =
   SELECT * FROM @a
   EXCEPT ALL
   SELECT * FROM @b;

@b_except_a_all =
   SELECT * FROM @b
   EXCEPT ALL
   SELECT * FROM @a;
```


@a_except_b_all

| DepID | name |
| --- | --- |
| 3 | Case |



@b_except_a_all

| DepID | name |
| --- | --- |
| 1 | Smith |
| 4 | Dey |
| 4 | Dey |





```
@a_except_b_distinct =
   SELECT * FROM @a
   EXCEPT DISTINCT
   SELECT * FROM @b;

@b_except_a_distinct =
   SELECT * FROM @b
   EXCEPT DISTINCT
   SELECT * FROM @a;
```

@a_except_b_distinct

| DepID | name |
| --- | --- |
| 3 | Case |


@b_except_a_distinct

| DepID | name |
| --- | --- |
| 4 | Dey |
