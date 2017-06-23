
# Reporting aggregation functions

Window functions also support the following aggregates:

- COUNT
- SUM
- MIN
- MAX
- AVG
- STDEV
- VAR

The syntax:

&lt;AggregateFunction&gt;( [DISTINCT] &lt;expression&gt;) [&lt;OVER\_clause&gt;]

Note:

- By default, aggregate functions, except COUNT, ignore null values.
- When aggregate functions are specified along with the OVER clause, the ORDER BY clause is not allowed in the OVER clause.

## SUM

The following example adds a total salary by department to each input row:

@result=

    SELECT

        \*,

        SUM(Salary) OVER( PARTITION BY DeptName ) AS TotalByDept

    FROM @employees;

| **EmpID** | **EmpName** | **DeptName** | **DeptID** | **Salary** | **TotalByDept** |
| --- | --- | --- | --- | --- | --- |
| 1 | Noah | Engineering | 100 | 10000 | 60000 |
| 2 | Sophia | Engineering | 100 | 20000 | 60000 |
| 3 | Liam | Engineering | 100 | 30000 | 60000 |
| 7 | Mason | Executive | 300 | 50000 | 50000 |
| 4 | Emma | HR | 200 | 10000 | 30000 |
| 5 | Jacob | HR | 200 | 10000 | 30000 |
| 6 | Olivia | HR | 200 | 10000 | 30000 |
| 8 | Ava | Marketing | 400 | 15000 | 25000 |
| 9 | Ethan | Marketing | 400 | 10000 | 25000 |

## COUNT

The following example adds an extra field to each row to show the total number employees in each department.

@result =

    SELECT \*,

        COUNT(\*) OVER(PARTITION BY DeptName) AS CountByDept

    FROM @employees;

| **EmpID** | **EmpName** | **DeptName** | **DeptID** | **Salary** | **CountByDept** |
| --- | --- | --- | --- | --- | --- |
| 1 | Noah | Engineering | 100 | 10000 | 3 |
| 2 | Sophia | Engineering | 100 | 20000 | 3 |
| 3 | Liam | Engineering | 100 | 30000 | 3 |
| 7 | Mason | Executive | 300 | 50000 | 1 |
| 4 | Emma | HR | 200 | 10000 | 3 |
| 5 | Jacob | HR | 200 | 10000 | 3 |
| 6 | Olivia | HR | 200 | 10000 | 3 |
| 8 | Ava | Marketing | 400 | 15000 | 2 |
| 9 | Ethan | Marketing | 400 | 10000 | 2 |

## MIN and MAX

The following example adds an extra field to each row to show the lowest salary of each department:

@result =

    SELECT

        \*,

        MIN(Salary) OVER ( PARTITION BY DeptName ) AS MinSalary

    FROM @employees;

| **EmpID** | **EmpName** | **DeptName** | **DeptID** | **Salary** | **MinSalary** |
| --- | --- | --- | --- | --- | --- |
| 1 | Noah | Engineering | 100 | 10000 | 10000 |
| 2 | Sophia | Engineering | 100 | 20000 | 10000 |
| 3 | Liam | Engineering | 100 | 30000 | 10000 |
| 7 | Mason | Executive | 300 | 50000 | 50000 |
| 4 | Emma | HR | 200 | 10000 | 10000 |
| 5 | Jacob | HR | 200 | 10000 | 10000 |
| 6 | Olivia | HR | 200 | 10000 | 10000 |
| 8 | Ava | Marketing | 400 | 15000 | 10000 |
| 9 | Ethan | Marketing | 400 | 10000 | 10000 |
