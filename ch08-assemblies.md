# Assemblies

First, we'll show you what it looks like when a U-SQL script uses an assembly. Then we'll show you all the steps needed to achieve this goal.


```
REFERENCE ASSEMBLY MyDB.OrdersLibAsm;

@customers  = 
  SELECT * FROM 
    (VALUES
       ("Contoso",   123 ),
       ("Woodgrove", 456 )
    ) AS D( Customer, Id  );

@customers =
  SELECT 
    OrdersLib.Helpers.Normalize(Customer) AS Customer, 
    Id
  FROM @customers;
```

Things to notice:
* The `REFERENCE ASSEMBLY` statement makes the code from the assembly named `OrdersLibAsm` to the script.
* The identifier `OrdersLibAsm` is the name the U-SQL catalog uses for the assembly. It is independent of the assembly filename.
* The identifier `OrdersLib` is the namespace in the `OrdersLibAsm` assembly that contains a static class called `Helpers` which in turn contains a static method called `Normalize`.


# Step 1 creating a .NET assembly

Create a .NET assembly with a filename of `OrdersLib.dll` with this code

```
namespace OrdersLib { 
  public static class Helpers { 
    public static string Normalize(string s) 
    { return s.ToLower(); }
  }
}
```

# Step 2 Upload the assembly into the default ADLS store of the ADLA account

For example place the assembly in this location

```
"/DLLs/OrdersLib.dll"
```

# Register the assembly in the catalog

For example place the assembly in this location

```
CREATE ASSEMBLY MyDB.OrdersLibAsm
  FROM @"/DLLs/OrdersLib.dll"; 
```

The dll has been copied to the U-SQL database called MyDB. You can now safely delete the file from `"/DLLs/OrdersLib.dll"`.


# Run a script that uses the assembly

```
REFERENCE ASSEMBLY MyDB.OrdersLibAsm;

@customers  = 
  SELECT * FROM 
    (VALUES
       ("Contoso",   123 ),
       ("Woodgrove", 456 )
    ) AS D( Customer, Id  );

@customers =
  SELECT 
    OrdersLib.Helpers.Normalize(Customer) AS Customer, 
    Id
  FROM @customers;
```

