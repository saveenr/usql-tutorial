# Assign Globally Unique Row Number

It's often useful to assign a globally unique number to each row. This is easy (and more efficient than using a reducer) with the ranking functions.

```
@result =
SELECT
*,
ROW_NUMBER() OVER () AS RowNumber
FROM @querylog;
```
