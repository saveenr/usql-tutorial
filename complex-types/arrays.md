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

