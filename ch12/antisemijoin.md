#ANTISEMIJOIN

An ANTISEMIJOIN is well .. .the opposite of SEMIJOIN

There are two variants: LEFT ANTISEMIJOIN and RIGHT ANTI SEMIJOIN.
* LEFT SEMIJOIN -> Give only those rows in the left rowset that DO NOT have a matching row in the right rowset.
* RIGHT SEMIJOIN -> Give only those rows in the right rowset that DO NOT have a matching row in the left rowset.

SQL has an ANTIJOIN (http://en.wikipedia.org/wiki/Relational_algebra#Antijoin) operator, but U-SQL does not.


Find all departments that don’t have an employee listed in the employees rowset

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

