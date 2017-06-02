## Native Data types

U-SQL has many built-in Native U-SQL datatypes in that U-SQL has special support for them so that using them will be more performant. In general use the U-SQL's native data types wherever possible.

Most of the datatypes below will be familiar to a C\#/.NET programmer. Indeed except for MAP and ARRAY, these _ARE_ the normal .NET datatypes you are used to.

**Miscellaneous**

```
bool
Guid
DateTime
byte[]
```

**Numeric signed**

```
sbyte
int
long
float
double
decimal
short
```

**Numeric unsigned**

```
byte
uint
ulong
ushort
```

**Text**

```
char
string
```

**Complex**

```
MAP<k,v>
ARRAY<v>
```

## Nullable Data Types

In .NET types that cannot be null such as `int` are called **value types**. Types that can have a null value such as string are called **reference** types. Sometimes it is convenient though to value that is an value type but that that can also have a null value. These types are called **nullable types**.

So if this is how we would use an int

```
int i = 100;
i = 200;
```

This is how we would use a nullable int.

```
int? i = 100;
i = 200;
i = null;
```

U-SQL supports the following nullable types

```
byte?
sbyte?
int?
uint?
long?
ulong?
float?
double?
decimal?
short?
ushort?
bool?
Guid?
DateTime?
char?
```

## 



