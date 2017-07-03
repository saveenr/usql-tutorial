## Referencing assemblies in the U-SQL Catalog of other ADLA accounts

If the assembly you want is in another ADLA account, and you have access to that assembly you can reference it as shown below.

```
REFERENCE SYSTEM ASSEMBLY [Account][DBName].[AssemblyName];
```