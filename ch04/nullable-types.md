# Nullable Data Types

In .NET types that cannot be null such as `int` are called **value types**. Types that can have a null value such as string are called **reference** types. Sometimes it is convenient though to value that is an value type but that that can also have a null value. These types are called **nullable types**.

So if this is how we would use an int in C#.

```
// This is C# code, not U-SQL
int i = 100;
i = 200;
```

This is how we would use a nullable int.

```
// This is C# code, not U-SQL
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



