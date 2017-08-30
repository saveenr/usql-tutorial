# The U-SQL Catalog

The U-SQL Catalog is the way U-SQL organizes data and code for re-use and sharing.

## The U-SQL Catalog metadata system

```
ADLA Account
|
|--Catalog
   |
   |--Databases
      |
      |--Schemas
      |  |--Tables
      |  |--Views      
      |  |--Table-valued functions
      |  |--Procedures
      |  |--Assemblies
      |  |--Credentials
      |  |--External Data Sources
      |
      |--Assemblies
```

* Every ADLA account has a single U-SQL catalog. The catalog cannot be deleted.
* Each U-SQL catalog contains one or more U-SQL databases. 
* Every catalog has a `master` database that cannot be deleted.
* Each U-SQL database can contain code and data. 
* Data is stored in the form of U-SQL tables. 
* U-SQL code is stored in a database in the form of views, table-valued functions, procedures. 
* .NET code \(.NET assemblies\) is stored in the database in the form of U-SQL assemblies.



