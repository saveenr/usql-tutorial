## SEMIJOIN
It is easiest to think of SEMIJOIN as a way to filter a rowset.
There are two variants: LEFT SEMIJOIN and RIGHT SEMIJOIN.

* LEFT SEMIJOIN -> Give only those rows in the left rowset that have a matching row in the right rowset.
* RIGHT SEMIJOIN -> Give only those rows in the right rowset that have a matching row in the left rowset.

NOTE: If you leave out LEFT or RIGHT, and instead simply write SEMIJOIN then what you get is LEFT SEMIJOIN. Do not leave out LEFT or RIGHT always explicitly it.

**Find all employees that are in valid departments**

```
@left_semijoin1 =
    SELECT
        @employees.DepID,    
        @employees.EmpName
    FROM @employees
    LEFT SEMIJOIN @departments
        ON @employees.DepID == @departments.DepID;
```

| DepID | EmpName |
| --- | --- |
| 31 | Rafferty |
| 33 | Jones |
| 33 | Heisenberg |
| 34 | Robinson |
| 34 | Smith |


Find all departments that has an employee listed in the employee rowset

```
@left_semijoin2 =
    SELECT 
        @departments.DepID,
        @departments.DepName
    FROM @departments
    LEFT SEMIJOIN @employees
    ON @departments.DepID == @employees.DepID;
```

| DepID | DepName |
| --- | --- |
| 31 | Sales |
| 33 | Engineering |
| 34 | Clerical |


