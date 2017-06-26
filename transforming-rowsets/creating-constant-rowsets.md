# Creating constant RowSets

RowSets can be directly created in a U-SQL script. 

```
@departments =
  SELECT * 
  FROM (VALUES
      (31, "Sales"),
      (33, "Engineering"),
      (34, "Clerical"),
      (35, "Marketing")
    ) AS D( DepID, DepName );
```


| DepID | DepName |
| ----- | -------- |
| 31 | Sales |
| 33| Engineering |
| 34 | Clerical |
| 35 | Marketing |

