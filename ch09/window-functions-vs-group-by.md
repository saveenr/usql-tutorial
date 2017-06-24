# Compare window functions to Grouping

Windowing and Grouping are conceptually related by also different. It is helpful to understand this relationship.

## Use aggregation and Grouping

The following query uses an aggregation to calculate the total salary for all employees:

```
@result = 
    SELECT 
        SUM(Salary) AS TotalSalary
    FROM @employees;
```

The result is a single row with a single column. The $165000 is the sum of of the Salary value from the whole table.

```
TotalSalary
165000
```

The following statement use the GROUP BY clause to calculate the total salary for each department:

```
@result=
    SELECT DeptName, SUM(Salary) AS SalaryByDept
    FROM @employees
    GROUP BY DeptName;
```

The results are:

```
DeptName	SalaryByDept
Engineering	60000
HR	30000
Executive	50000
Marketing	25000
```

The sum of the SalaryByDept column is $165000, which matches the amount in the last script.
In both these cases the number of there are fewer output rows than input rows:
Without GROUP BY, the aggregation collapses all the rows into a single row.
With GROUP BY, there are N output rows where N is the number of distinct values that appear in the data, In this case, you will get 4 rows in the output.

## Use a window function

The OVER clause in the following sample is empty. This defines the "window" to include all rows. The SUM in this example is applied to the OVER clause that it precedes.
You could read this query as: “The sum of Salary over a window of all rows”.

```
@result=
    SELECT
        EmpName,
        SUM(Salary) OVER( ) AS SalaryAllDepts
    FROM @employees;
```

Unlike GROUP BY, there are as many output rows as input rows:

```
EmpName	TotalAllDepts
Noah	165000
Sophia	165000
Liam	165000
Emma	165000
Jacob	165000
Olivia	165000
Mason	165000
Ava	165000
Ethan	165000
```

The value of 165000 (the total of all salaries) is placed in each output row. That total comes from the "window" of all rows, so it includes all the salaries.
The next example demonstrates how to refine the "window" to list all the employees, the department, and the total salary for the department. PARTITION BY is added to the OVER clause.

```
@result=
SELECT
    EmpName, 
    DeptName,
    SUM(Salary) OVER( PARTITION BY DeptName ) AS SalaryByDept
FROM @employees;
```

The results are:

```
EmpName	DeptName	SalaryByDep
Noah	Engineering	60000
Sophia	Engineering	60000
Liam	Engineering	60000
Mason	Executive	50000
Emma	HR	30000
Jacob	HR	30000
Olivia	HR	30000
Ava	Marketing	25000
Ethan	Marketing	25000
```

Again, there are the same number of input rows as output rows. However each row has a total salary for the corresponding department.
