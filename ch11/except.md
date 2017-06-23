
# Finding Rows That Are NOT in the Other RowSet with EXCEPT

The **EXCEPT** operator returns all the rows in the left RowSet that _are not_ in the right RowSet.

```
@except\_all\_ab =

SELECT \* FROM @a

EXCEPT DISTINCT

SELECT \* FROM @b;

@except\_distinct\_ab =

SELECT \* FROM @a

EXCEPT ALL

SELECT \* FROM @b;

@except\_all\_ba =

SELECT \* FROM @b

EXCEPT DISTINCT

SELECT \* FROM @a;

@except\_dstinct\_ba =

SELECT \* FROM @b

EXCEPT ALL

SELECT \* FROM @a;
```

EXCEPT ALL (A,B)

| DepID | name |
| --- | --- |
| 3 | Case |


EXCEPT DISTINCT (A,B)

| DepID | name |
| --- | --- |
| 3 | Case |


EXCEPT ALL (B,A)

| DepID | name |
| --- | --- |
| 1 | Smith |
| 4 | Dey |
| 4 | Dey |


EXCEPT DISTINCT (B,A)

| DepID | name |
| --- | --- |
| 4 | Dey |
