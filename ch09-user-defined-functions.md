# User-Defined Functions

User defined functions are normal static methods on a .NET Class 


Compile the code below as OrdersLib.dll and upload it to "/DLLs/OrdersLib.dll".

```
namespace OrdersLib
{
  public static class Helpers
  {
    public static string Normalize(string s)
    {
      return s.ToLower();
    }
  }
}
```

Then register it in the U-SQL Catalog

```
CREATE DATABASE IF NOT EXISTS MyDB;
CREATE ASSEMBLY MyDB.OrdersLibAsm FROM @"/DLLs/OrdersLib.dll";
```

Now you can use it in a U-SQL script.

```
REFERENCE ASSEMBLY MyDB.OrdersLibAsm;

@customers =
  SELECT * FROM
    (VALUES
      ("Contoso", 123 ),
      ("Woodgrove", 456 )
    ) AS D( Customer, Id );

@customers =
  SELECT
    OrdersLib.Helpers.Normalize(Customer) AS Customer,
    Id
  FROM @customers;
```
