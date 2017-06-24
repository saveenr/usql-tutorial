# Sample Datasets

## QueryLog sample dataset

QueryLog represents a list of what people searched for in search engine. Each query log includes:
* Query - What the user was searching for.
* Latency - How fast the query came back to the user in milliseconds.
* Vertical - What kind of content the user was interested in (Web links, Images, Videos).
Copy and paste the following script into your U-SQL project for constructing the QueryLog rowset:


```
@querylog = 
    SELECT * FROM ( VALUES
        ("Banana"  , 300, "Image" ),
        ("Cherry"  , 300, "Image" ),
        ("Durian"  , 500, "Image" ),
        ("Apple"   , 100, "Web"   ),
        ("Fig"     , 200, "Web"   ),
        ("Papaya"  , 200, "Web"   ),
        ("Avocado" , 300, "Web"   ),
        ("Cherry"  , 400, "Web"   ),
        ("Durian"  , 500, "Web"   ) )
    AS T(Query,Latency,Vertical);
```

## Employees sample dataset

The Employee dataset includes the following fields:

* EmpID - Employee ID
* EmpName  Employee name
* DeptName - Department name
* DeptID - Deparment ID
* Salary - Employee salary

Copy and paste the following script into your U-SQL project for constructing the Employees rowset:

```
@employees = 
    SELECT * FROM ( VALUES
        (1, "Noah",   "Engineering", 100, 10000),
        (2, "Sophia", "Engineering", 100, 20000),
        (3, "Liam",   "Engineering", 100, 30000),
        (4, "Emma",   "HR",          200, 10000),
        (5, "Jacob",  "HR",          200, 10000),
        (6, "Olivia", "HR",          200, 10000),
        (7, "Mason",  "Executive",   300, 50000),
        (8, "Ava",    "Marketing",   400, 15000),
        (9, "Ethan",  "Marketing",   400, 10000) )
    AS T(EmpID, EmpName, DeptName, DeptID, Salary);
```

