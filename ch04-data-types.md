Native Data types
U-SQL has many built-in Native U-SQL datatypes in that U-SQL has special support for them so that using them will be more performant. In general use the U-SQL's native data types wherever possible.

Most of the datatypes below will be familiar to a C#/.NET programmer. Indeed except for MAP and ARRAY, these *ARE* the normal .NET datatypes you are used to.
		
Miscellaneous	


```
bool
Guid
DateTime
byte[]
```




Numeric	



```
byte
sbyte
int
uint
long
ulong
float
double
decimal
short
ushort

```




Text	

```
char
string
```

char?

		
Complex	



```
MAP<k,v>
ARRAY<v>	
```


##Nullable Data Types

For those of you wondering what the question marks mean (int?) please read this article on nullable types.




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



## String size limitations

A string in a rowset can only be 128K in size.
If you need to store something larger than that, use byte [].
NOTE: In the future this size limitation may change



