# ANTISEMIJOIN

An **ANTISEMIJOIN** is the opposite of **SEMIJOIN**

There are two variants:
* **LEFT ANTISEMIJOIN** -> Give only those rows in the left RowSet that DO NOT have a matching row in the right rowset.
* **RIGHT ANTISEMIJOIN** -> Give only those rows in the right RowSet that DO NOT have a matching row in the left RowSet.

Find all departments that don’t have an employee listed in the employees RowSet.

```
@left_antisemijoin =
    SELECT 
        @departments.DepID,
        @departments.DepName
    FROM @departments
    LEFT ANTISEMIJOIN @employees
        ON @departments.DepID == @employees.DepID;
```

| DepID | DepName |
| --- | --- |
| 35 | Marketing |

