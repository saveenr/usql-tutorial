
# ARRAYs 

## Creating ARRAYs

### Creating Empty arrays

```
DECLARE @m = new SqlArray<string> ( );
```
or 

```
DECLARE @m = new SqlArray<string> { };
```

### Creating arrays initialized with data

```
DECLARE @m = new SqlArray<string> { "A", "B" };
```




# MAPS

## Creating MAPs 

### Creating empty maps

```
DECLARE @m = new SqlMap<string, string> { };
```

or

```
DECLARE @m = new SqlMap<string, string> { };
```

## Creating maps initialized with data

```
DECLARE @m = new SqlMap<string, string> 
    { 
        {"Seattle", "1"}, 
        {"Portland", "2"} 
    };
```