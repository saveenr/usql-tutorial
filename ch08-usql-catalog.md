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

## Reusing U-SQL code with a table-valued function \(TVF\)

Many of the scripts in the tutorial have required reading from the searchlog and the code to read from the searchlog is shown below

```
@searchlog =    
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    FROM "/SearchLog.tsv"
    USING Extractors.Tsv();
```

Now instead of writing this code over and over in every script we will store the code as a TVF in a database. The name of this function is going to be `MyDB.dbo.ExtractSearchLog`. The "dbo" part of that name is the "schema".



```
CREATE FUNCTION MyDB.dbo.ExtractSearchLog()
RETURNS @rows 
AS BEGIN  
  @rows = 
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    FROM "/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
END;
```

Notice that CREATE FUNCTION indicates it will return a RowSet call @rows. Then in the @rows RowSet is defined.

Now that the TVF is created, we can call it this way.

```
@searchlog = MyDB.dbo.ExtractSearchLog();
```

## 

## Deleting a Database

DROP DATABASE MyDB;

DROP DATABASE IF EXISTS MyDB;

