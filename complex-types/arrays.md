# ARRAYs

## Overview

* An ordered list of values (all of the same type)
* Immutable
* Single-Dimension

## Sample data




## Creating ARRAYs

### Creating Empty arrays

```
DECLARE @arr = new SqlArray<string> ( );
```
or

```
DECLARE @arr = new SqlArray<string> { };
```

### Creating arrays initialized with data

```
DECLARE @arr = new SqlArray<string> { "A", "B" };
```

