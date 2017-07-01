# Supported datatypes

### Text

```
DECLARE @text1 string = "Hello World";
DECLARE @text2 string = @"Hello World";

DECLARE @text3 char   = 'a';
```


###  Datetimes

```
DECLARE @d1 DateTime = System.DateTime.Parse("1979/03/31");
DECLARE @d2 DateTime= DateTime.Now;
```

### Signed numerics

```
DECLARE @a sbyte  = 0;
DECLARE @b short  = 1;

DECLARE @c int    = 2;
DECLARE @d long   = 3L;

DECLARE @e float  = 4.0f;
DECLARE @f double = 5.0;
```

### Unsigned numerics

```
DECLARE @g byte   = 0;
DECLARE @h ushort = 1;
DECLARE @i uint   = 2;
DECLARE @j ulong  = 3L;
```

### .NET arrays

```
DECLARE @array1 byte [] = new byte[] { 0, 1, 2, 3, 4 };
DECLARE @array2 string [] = new string[] { "foo", "bar", "beer" };
```

### Complex types

```
DECLARE @m SqlMap<string, string> = new SqlMap<string, string> 
    { 
        {"This", "is a string in a map"}, 
        {"That", "is also a string in a map"} 
    };
```

### Miscellaneous

```
DECLARE @misc1 bool    = true;
DECLARE @misc2 Guid    = System.Guid.Parse("BEF7A4E8-F583-4804-9711-7E608215EBA6");
```





