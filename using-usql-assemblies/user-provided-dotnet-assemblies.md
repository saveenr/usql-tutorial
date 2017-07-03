# User-provided assemblies




## Referencing assemblies in the U-SQL Catalog

```
CREATE ASSEMBLY MyDB.OrdersLibAsm
  FROM @"/DLLs/OrdersLib.dll"; 
```

Below is the standard syntax for referencing an assembly that is in the U-SQL catalog.
 
```
REFERENCE SYSTEM ASSEMBLY [DBName].[AssemblyName];
```


## Simplifying code with the USING statement

Similar, to C#'s using statement, U-SQL lets you provide an alias for a namespace, this improve the readability of your code.

```
USING Exc = System.Exceptions;
```


