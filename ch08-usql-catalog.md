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
      |  |--Table-valued functions
      |
      |--Assemblies
```

* Every ADLA account has a single U-SQL Catalog. The catalog cannot be deleted.
* Each U-SQL Catalog contains one or more U-SQL Databases. Every catalog has a `master` database that cannot be deleted.
* Each U-SQL Database can contain code and data. 
  * Data is stored in the form of U-SQL tables. 
  * U-SQL code is stored in a database in the form of Views, Table-valued functions, Procedures. 
  * .NET code is stored in the database in the form on assemblies.

## Creating a database

To create a database is simple.

```
CREATE DATABASE MyDB;
```


This command will fail if the database already exists. Often, it will be the case that you want to create a database only if it does not exist. In this case the following command is used.

```
CREATE DATABASE IF NOT EXISTS MyDB;
```


## Deleting a Database

DROP DATABASEMyDB;

DROP DATABASE IF EXISTSMyDB;





