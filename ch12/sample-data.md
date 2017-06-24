# Sample data

```
@departments  =
    SELECT * FROM
        (VALUES
            ("31", "Sales"),
            ("33", "Engineering"),
            ((int?)34, "Clerical"),
            ((int?)35, "Marketing")
        ) AS D( DepID, DepName );
```


```
@employees  =
    SELECT * FROM
        (VALUES
            ((int?)31, "Rafferty"),
            ((int?)33, "Jones"),
            ((int?)33, "Heisenberg"),
            ((int?)34, "Robinson"),
            ((int?)34, "Smith"),
            ((int?)null, "Williams")
        ) AS D( DepID, EmpName );
```


# CROSS JOIN

```
@cross_join =
    SELECT
        @departments.DepID AS DepID_Dep,
        @employees.DepID AS DepID_Emp,
        @employees.EmpName, 
        @departments.DepName
    FROM @employees CROSS JOIN @departments;
```

|DepIDDep|DepIDEmp|EmpName|DepName|
|---|---|---|---|
|31|31|Rafferty|Sales|
|31|31|Rafferty|Sales|
﻿|31|33|Jones|Sales|
﻿|31|33|Heisenberg|Sales|
﻿|31|34|Robinson|Sales|
﻿|31|34|Smith|Sales|
﻿|31|ERR|Williams|Sales|
﻿|33|31|Rafferty|Engineering|
﻿|33|33|Jones|Engineering|
﻿|33|33|Heisenberg|Engineering|
﻿|33|34|Robinson|Engineering|
﻿|33|34|Smith|Engineering|
﻿|33|ERR|Williams|Engineering|
﻿|34|31|Rafferty|Clerical|
﻿|34|33|Jones|Clerical|
﻿|34|33|Heisenberg|Clerical|
﻿|34|34|Robinson|Clerical|
﻿|34|34|Smith|Clerical|
﻿|34|ERR|Williams|Clerical|
﻿|35|31|Rafferty|Marketing|
﻿|35|33|Jones|Marketing|
﻿|35|33|Heisenberg|Marketing|
﻿|35|34|Robinson|Marketing|
﻿|35|34|Smith|Marketing|
﻿|35|NULL|Williams|Marketing|






