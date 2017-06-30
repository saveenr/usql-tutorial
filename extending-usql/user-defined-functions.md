# User-Defined Functions

User defined functions are normal static methods on a .NET Class. Below is the code for a simple class.

```
namespace OrdersLib
{
  public static class Helpers
  {
    public static string Normalize(string s)
    { return s.ToLower(); }
  }
}
```

Assuming you registered the assembly that contains this class using the name "MyDB.OrdersLibAsm", you can use the assembly and the UDF as shown below.

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
