## Numbering Rows

Using the **ROW\_NUMBER** windowing function aggregate is how to assign row numbers. ROW\_NUMBER is part of Windowing Functions and that topic too complex for this tutorial – See the Windowing Functions documentation for details. However, for now we do want to show you the proper way to number rows in U-SQL using ROW\_NUMBER because it is a popular topic.

```
@rs1 =
 SELECT
   ROW_NUMBER() OVER ( ) AS RowNumber,
   Start,
   Region
 FROM @searchlog;
```



