# Persisting UDTs

UDTs cannot be persistent directly into a U-SQL table. You'll have to manually convert your UDT's value to a U-SQL-supported datataype. 


# Persisting UDTs into a file 


```
@products2 = 
    SELECT 
        ProductCode
        BitString, 
        Bits.ToInteger() AS BitInt
    FROM @products;
```

Now it can be persisted into a text file:

```
OUTPUT @products2
    TO "/output.csv"
    USING Outputters.Csv();
```

# Persisting UDTs into a table file 


```
CREATE TABLE MyDB.dbo.MyTable
( 
    INDEX idx  
    CLUSTERED(ProductCode ASC)
    DISTRIBUTED BY HASH(ProductCode) 
) AS SELECT * FROM @products2;
```

