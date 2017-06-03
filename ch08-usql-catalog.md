The U-SQL Catalog

The U-SQL Catalog is the way U-SQL organizes data and code for re-use and sharing. 

The U-SQL metadata system

```
Account
|--Catalog
   |--Databases
      |--Schemas
         |--Tables
         |--Table-valued functions
      |--Assemblies

```


Every ADLA account has a single U-SQL Catalog.

Each U-SQL Catalog contains one or more U-SQL Databases. Every catalog has a master database that cannot be deleted.

Each U-SQL Database can contain code and data. Data is in the form of U-SQL tables. U-SQL code is stored in a database in the form of Views, Table-valued functions, Procedures. .NET code is stored in the database in the form on assemblies. 





