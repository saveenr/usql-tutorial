# U-SQL Tables

U-SQL tables offer a way to store data in form that preserves it schema and organize data to high-performance queries which is very important as data sizes grow.

# Creating a table from a RowSet

If you have a RowSet creating a table from it is very simple. The one thing you must remember is that a table must have an index defined. The table gets its schema from the schema of the rowset.

```
@customers  = 
  SELECT * FROM 
    (VALUES
       ("Contoso",   123 ),
       ("Woodgrove", 456 )
    ) AS D( Customer, Id  );

DROP TABLE IF EXISTS MyDB.dbo.Customers;

CREATE TABLE MyDB.dbo.Customers
( 
    INDEX idx  
    CLUSTERED(customer ASC)
    DISTRIBUTED BY HASH(customer) 
) AS SELECT * FROM @customers;

```

# Creating an empty table and filling it latter

If you need don't have the data available at the time of table creation. You can create an empty table as shown below. Notice that this time the schema has to be specified.

```
DROP TABLE IF EXISTS MyDB.dbo.Customers;

CREATE TABLE MyDB.dbo.Customers
( 
    Customer string, 
    Id int, 
    INDEX idx  
        CLUSTERED(Customer ASC)
        DISTRIBUTED BY HASH(Customer) 
);
```

Then separately, you can fill the table.

```
@customers  = 
  SELECT * FROM 
    (VALUES
       ("Contoso",   123 ),
       ("Woodgrove", 456 )
    ) AS D( Customer, Id  );

INSERT INTO MyDB.dbo.Customers
    SELECT * FROM @customers;
```

# Reading from a table

```
@rs = 
  SELECT * 
  FROM MyDB.dbo.Customers;
```
