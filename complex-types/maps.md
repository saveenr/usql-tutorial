# MAPs

### Overview




### Creating Empty maps

```
new SqlMap<string,string> ( )
```
or

```
new SqlMap<string,string> { }
```

### Creating maps initialized with data

```
new SqlMap<string,string> { {"A", "X"}, {"B", "Y"} }
```

```
@output =
    SELECT * 
    FROM  
    ( VALUES
        ( "cat1", new SqlMap<string,string> { {"A", "X"}, {"B", "Y"} } )

    )
AS T(Category, Dict);
```

| Category string | Dict SqlMap-2 |
| --- | --- |
| cat1 | SqlMap &lt; string, string &gt; { A=X; B=Y } |

### Sample data

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
| Website | SqlMap<string, string>{ Alice=Dev; Bob=Dev; Chris=UX; Mallory=PM; Stan=Dev } |
| DB | SqlMap<string, string>{ Chuck=Dev; Joe=Dev; Ted=Test } |


### Removing members based on keys

```
@output =
    SELECT Project,
           new SqlMap<string,string>(Members.Where(kv => kv.Key != "Mallory")) AS Members
    FROM @projectmembers;
```

| Project | Members |
| --- | --- |
| Website | SqlMap<string, string>{ Alice=Dev; Bob=Dev; Chris=UX; Stan=Dev } |
| DB | SqlMap<string, string>{ Chuck=Dev; Joe=Dev; Ted=Test } |


### Removing members based on values

```
@output =
    SELECT Project,
           new SqlMap<string,string>(Members.Where(kv => kv.Value != "Dev")) AS Members
    FROM @projectmembers;
```

| Project | Members |
| --- | --- |
| Website | SqlMap<string, string>{ Chris=UX; Mallory=PM } |
| DB | SqlMap<string, string>{ Ted=Test } |

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
| Website | SqlMap<string, string>{ Alice=Dev; Bob=Dev; Chris=UX; Mallory=PM; Stan=Dev } | 5 |
| DB | SqlMap<string, string>{ Chuck=Dev; Joe=Dev; Ted=Test } | 3 |

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


