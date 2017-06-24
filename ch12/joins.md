# Joins

https://en.wikipedia.org/wiki/Join_(SQL)



# INNER and OUTER JOINs

* An `INNER JOIN` of these two rowsets will return the combined rows where the Join Key matches.
* A `LEFT OUTER JOIN` is a superset of the `INNER JOIN` that includes ALL the rows from the left rowset. If the Join Key is not present in the right rowset, NULL values are used for that output row.
* A `RIGHT OUTER JOIN` is a superset of the `INNER JOIN` that includes ALL the rows from the right rowset. If the Join Key is not present in the right rowset, NULL values are used for that output row.
* A `FULL OUTER JOIN` is a combination of the `LEFT OUTER` and `RIGHT OUTER JOIN`
* Using `OUTER JOIN` without specifying `LEFT`, `RIGHT`, or `FULL` indicates a `LEFT OUTER JOIN`.

## INNER JOIN

```
@inner_join =  
    SELECT 
        @employees.DepID AS EmpDepId, @departments.DepID , @employees.EmpName, @departments.DepName
        FROM @employees 
        INNER JOIN @departments ON @employees.DepID == @departments.DepID;
```

| EmpDepId | DepID | EmpName | DepName |
| --- | --- | --- | --- |
| 33 | 33 | Jones | Engineering |
| 33 | 33 | Heisenberg | Engineering |
| 34 | 34 | Robinson | Clerical |
| 34 | 34 | Smith | Clerical |


```
@left_outer_join =  
    SELECT 
        @employees.DepID AS EmpDepId, @departments.DepID , @employees.EmpName, @departments.DepName
        FROM @employees 
        LEFT OUTER JOIN @departments ON @employees.DepID == @departments.DepID;
```

| EmpDepId | DepID | EmpName | DepName |
| --- | --- | --- | --- |
| 33 | 33 | Jones | Engineering |
| 33 | 33 | Heisenberg | Engineering |
| 34 | 34 | Robinson | Clerical |
| 34 | 34 | Smith | Clerical |
| NULL | NULL | Williams | NULL |

```
@right_outer_join =  
    SELECT 
        @employees.DepID AS EmpDepId, @departments.DepID , @employees.EmpName, @departments.DepName
        FROM @employees 
        RIGHT OUTER JOIN @departments ON @employees.DepID == @departments.DepID;
```

| EmpDepId | DepID | EmpName | DepName |
| --- | --- | --- | --- |
| 33 | 33 | Heisenberg | Engineering |
| 33 | 33 | Jones | Engineering |
| 34 | 34 | Smith | Clerical |
| 34 | 34 | Robinson | Clerical |
| NULL | 35 | NULL | Marketing |


```
@full_outer_join =  
    SELECT 
        @employees.DepID AS EmpDepId, @departments.DepID , @employees.EmpName, @departments.DepName
        FROM @employees 
        FULL OUTER JOIN @departments ON @employees.DepID == @departments.DepID;
```

| EmpDepId | DepID | EmpName | DepName |
| --- | --- | --- | --- |
| 33 | 33 | Jones | Engineering |
| 33 | 33 | Heisenberg | Engineering |
| 34 | 34 | Robinson | Clerical |
| 34 | 34 | Smith | Clerical |
| NULL | 35 | NULL | Marketing |
| NULL | NULL | Williams | NULL |
