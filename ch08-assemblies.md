# Assemblies

# Step 1 creating a .NET assembly

Create a .NET assembly with a filename of `OrdersLib.dll` with this code. In this case we will simply crate a single static string in the assembly that we will reuse in a U-SQL script.

```
namespace OrdersLib 
{ 
  public static class Helpers 
  { 
    public static string CustPrefix = "CUST_";
  }
}
```

# Step 2 Upload the assembly into the default ADLS store of the ADLA account

For example place the assembly in this location

```
"/DLLs/OrdersLib.dll"
```

# Step 3 Register the assembly in the catalog

For example place the assembly in this location

```
CREATE ASSEMBLY MyDB.OrdersLibAsm
  FROM @"/DLLs/OrdersLib.dll"; 
```

The identifier `OrdersLibAsm` is the name the U-SQL catalog uses for the assembly. It is independent of the assembly filename `"OrdersLib.dll"`.

The dll has been copied to the U-SQL database called MyDB. You can now safely delete the file from `"/DLLs/OrdersLib.dll"`.

# Step 4 Run a script that uses the assembly

The `REFERENCE ASSEMBLY` statement makes the code from the assembly named `OrdersLibAsm` to the script.

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
    (OrdersLib.Helpers.CustPrefix + Customer) AS Customer, 
    Id
  FROM @customers;
```

