# Nullable Data Types

In .NET types that cannot be null such as `int` are called **value types**. Types that can have a null value such as string are called **reference** types. Sometimes it is convenient though to value that is an value type but that that can also have a null value. These types are called **nullable types**.

## Using nullable types in C# code

```
// This is C# code, not U-SQL
int? i = 100;
i = 200;
i = null;
```

## Supported nullable types

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


## Nullable types don't apply to reference types such as `string`

The purpose of "nullable" is enable null values for a type that doesn't already support null values. The string type - like all .NET reference types - already supports null values.


