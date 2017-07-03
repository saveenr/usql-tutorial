## System assemblies

Some assemblies are part of the .NET Base Class Library. They aren't in the U-SQL catalog, but we still need a way to make them available to a U-SQL script. 

```
REFERENCE SYSTEM ASSEMBLY [System.Xml];
```

Notice that no database name is provided.
