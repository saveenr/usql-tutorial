# MAPs

## Overview



## Creating maps

### Creating Empty maps

```
DECLARE @map = new SqlMap<string,string> ( );
```
or

```
DECLARE @map = new SqlMap<string,string> { };
```

### Creating maps initialized with data

```
DECLARE @map = new SqlMap<string,string> { "A", "B" };
```

