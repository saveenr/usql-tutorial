# Escaping Column names

If you need column names that contain whitespace you can enclose the name in `[` and `]`.

The following script shows a column called **Order Number**.

```
@b =
  SELECT 
    [Order Number], 
    Part
  FROM @a;
```



