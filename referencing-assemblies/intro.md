# Referencing assemblies


## Referencing assemblies in the U-SQL Catalog

Below is the standard syntax for referencing an assembly that is in the U-SQL catalog.
 
```
REFERENCE SYSTEM ASSEMBLY [DBName].[AssemblyName];
```

## Referencing assemblies in the U-SQL Catalog of other ADLA accounts

If the assembly you want is in another ADLA account, and you have access to that assembly you can reference it as shown below.
 
```
REFERENCE SYSTEM ASSEMBLY [Account][DBName].[AssemblyName];
```

## Referencing system assemblies

Some assemblies are part of the .NET Base Class Library. They aren't in the U-SQL catalog, but we still need a way to make them available to a U-SQL script. 

```
REFERENCE SYSTEM ASSEMBLY [System.Xml];
```

## Simplifying code with the USING statement

Similar, to C#'s using statement, U-SQL lets you provide an alias for a namespace, this improve the readability of your code.

```
USING Exc = System.Exceptions;
```


