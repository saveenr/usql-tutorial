## Native Data types

U-SQL has many built-in Native U-SQL datatypes in that U-SQL has special support for them so that using them will be more performant. In general use the U-SQL's native data types wherever possible.

Most of the datatypes below will be familiar to a C\#/.NET programmer. Indeed except for MAP and ARRAY, these _ARE_ the normal .NET datatypes you are used to.



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

**Miscellaneous**

```
bool
Guid
DateTime
byte[]
```


