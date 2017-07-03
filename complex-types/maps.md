# MAPs

## Overview



## Creating maps

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
