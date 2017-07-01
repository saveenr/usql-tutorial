# Parameter type inference

DECLARE can infer the type as shown below.

```
DECLARE @a = "Hello World"; // string
DECLARE @b = 'a'; // char
DECLARE @c = 2; // int
DECLARE @d = 2L; // long
DECLARE @d = 4.0f; // float
DECLARE @e = 5.0; // double

DECLARE @m = new SqlMap<string, string> 
    {
        {"This", "is a string in a map"},
        {"That", "is also a string in a map"}
    };
```

