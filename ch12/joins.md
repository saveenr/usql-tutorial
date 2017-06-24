# Joins

https://en.wikipedia.org/wiki/Join_(SQL)

## CROSS JOIN

```
@cross_join =
    SELECT
        @departments.DepID AS DepID_Dep,
        @employees.DepID AS DepID_Emp,
        @employees.EmpName, 
        @departments.DepName
    FROM @employees CROSS JOIN @departments;
```    
    
| DepID_Dep | DepID_Emp | EmpName | DepName |
| --- | --- | --- | --- |
| 31 | 33 | Jones | Sales |
| 31 | 33 | Heisenberg | Sales |
| 31 | 34 | Robinson | Sales |
| 31 | 34 | Smith | Sales |
| 31 | NULL | Williams | Sales |
| 33 | 31 | Rafferty | Engineering |
| 33 | 33 | Jones | Engineering |
| 33 | 33 | Heisenberg | Engineering |
| 33 | 34 | Robinson | Engineering |
| 33 | 34 | Smith | Engineering |
| 33 | NULL | Williams | Engineering |
| 34 | 31 | Rafferty | Clerical |
| 34 | 33 | Jones | Clerical |
| 34 | 33 | Heisenberg | Clerical |
| 34 | 34 | Robinson | Clerical |
| 34 | 34 | Smith | Clerical |
| 34 | NULL | Williams | Clerical |
| 35 | 31 | Rafferty | Marketing |
| 35 | 33 | Jones | Marketing |
| 35 | 33 | Heisenberg | Marketing |
| 35 | 34 | Robinson | Marketing |
| 35 | 34 | Smith | Marketing |
| 35 | NULL | Williams | Marketing |


## SEMIJOIN
It is easiest to think of SEMIJOIN as a way to filter a rowset.
There are two variants: LEFT SEMIJOIN and RIGHT SEMIJOIN.

* LEFT SEMIJOIN -> Give only those rows in the left rowset that have a matching row in the right rowset.
* RIGHT SEMIJOIN -> Give only those rows in the right rowset that have a matching row in the left rowset.

NOTE: If you leave out LEFT or RIGHT, and instead simply write SEMIJOIN then what you get is LEFT SEMIJOIN. Do not leave out LEFT or RIGHT always explicitly it.
Find all employees that are in valid departments

```
@left_semijoin =
    SELECT 
        @employees.DepID,
        @employees.EmpName
    FROM @employees
    LEFT SEMIJOIN @departments
        ON @employees.DepID == @departments.DepID;
```


| DepID | EmpName |
| --- | --- |
| 33 | Jones |
| 33 | Heisenberg |
| 34 | Robinson |
| 34 | Smith |
