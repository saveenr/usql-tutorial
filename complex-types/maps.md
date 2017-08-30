# MAPs

### Overview



### Creating a SqlMap from constant values

The following snippet creates two rows and each has a SqlMap<K,V> where the key (K) is a string and the value is also a string.

```
@projectmembers = 
    SELECT *
    FROM
    ( VALUES
        ( "Website", new SqlMap<string,string> { 
                {"Mallory", "PM"}, 
                {"Bob", "Dev"} ,
                {"Alice", "Dev"} ,
                {"Stan", "Dev"} ,
                {"Chris", "UX"} ,
             } 
        ),
        ( "DB", new SqlMap<string,string> { 
                {"Ted", "Test"}, 
                {"Joe", "Dev"} ,
                {"Chuck", "Dev"} 
             } 
        )
)
AS T(Project, Members);
```

| Project | Members |
| --- | --- |
| Website | SqlMap{ Alice=Dev; Bob=Dev; Chris=UX; Mallory=PM; Stan=Dev } |
| DB | SqlMap{ Chuck=Dev; Joe=Dev; Ted=Test } |




### Creating Empty maps

```
new SqlMap<string,string> ( )
```
or

```
new SqlMap<string,string> { }
```


### Maps from Maps: Removing members based on keys

```
@output =
    SELECT Project,
           new SqlMap<string,string>(Members.Where(kv => kv.Key != "Mallory")) AS Members
    FROM @projectmembers;
```

| Project | Members |
| --- | --- |
| Website | SqlMap{ Alice=Dev; Bob=Dev; Chris=UX; Stan=Dev } |
| DB | SqlMap{ Chuck=Dev; Joe=Dev; Ted=Test } |

Alternatively you can use the SqlMap.Create static method instead

```
@output =
    SELECT Project,
           SqlMap.Create(Members.Where(kv => kv.Key != "Mallory")) AS Members
    FROM @projectmembers;
```

| Project | Members |
| --- | --- |
| Website | SqlMap{ Alice=Dev; Bob=Dev; Chris=UX; Stan=Dev } |
| DB | SqlMap{ Chuck=Dev; Joe=Dev; Ted=Test } |


### Combining rows into maps with MAP_AGG

```
@projectmembers = 
    SELECT *
    FROM
    ( VALUES
        ( "Website","Mallory", "PM" ), 
        ( "Website","Bob", "Dev" ),
        ( "Website","Alice", "Dev" ) ,
        ( "Website","Stan", "Dev" ) ,
        ( "Website","Chris", "UX" ) ,
        ( "DB", "Ted", "Test" ), 
        ( "DB", "Joe", "Dev" ) ,
        ( "DB", "Chuck", "Dev" ) 
    )
AS T(Project, Employee, Role);

@projectmembers =
    SELECT Project,
           MAP_AGG<string, string>(Employee, Role) AS Members
    FROM @projectmembers_raw
    GROUP BY Project;    
```
| Project | Members |
| --- | --- |
| DB | SqlMap{ Chuck=Dev; Joe=Dev; Ted=Test } |
| Website | SqlMap{ Alice=Dev; Bob=Dev; Chris=UX; Mallory=PM; Stan=Dev } |


### Removing members based on values

```
@output =
    SELECT Project,
           SqlMap.Create(Members.Where(kv => kv.Value != "Dev")) AS Members
    FROM @projectmembers;
```

| Project | Members |
| --- | --- |
| Website | SqlMap{ Chris=UX; Mallory=PM } |
| DB | SqlMap{ Ted=Test } |

### counting members

```
@output =
    SELECT Project,
           Members,
           Members.Count AS Count
FROM @projectmembers;
```

| Project | Members | Count |
| --- | --- | --- |
| Website | SqlMap{ Alice=Dev; Bob=Dev; Chris=UX; Mallory=PM; Stan=Dev } | 5 |
| DB | SqlMap{ Chuck=Dev; Joe=Dev; Ted=Test } | 3 |

### Retrieving values
```
@output =
    SELECT 
        Project,
        Members["Mallory"] AS MalloryRole
   FROM @projectmembers;
```

Note that if the key is missing then the default value for the type is returned

| Project | MalloryRole  |
| --- | --- | --- |
| Website | PM |
| DB | null |

### Checking if a key exists

```
@output =
    SELECT 
        Project,
        Members.ContainsKey("Mallory") AS ContainsMallory
FROM @projectmembers;
``` 

| Project | ContainsMallory |
| --- | --- |
| Website | True |
| DB | False |


