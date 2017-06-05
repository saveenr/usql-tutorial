## Type Inference

DECLARE can infer the type as shown below.

```
DECLARE @inputfile= "/data.csv"

@rows =
  EXTRACT name string, idint
  FROM @inputfile
  USING Extractors.Csv();
```

Examples of type inference.

```
DECLARE @a = "Hello World"; // string
DECLARE @b = 'a'; // char
DECLARE @c = 2; // int
DECLARE @d = 2L; // long
DECLARE @d = 4.0f; // float
DECLARE @e = 5.0; // double
```



