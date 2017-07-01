# Using Expressions with DECLARE

DECLARE parameters can be expressions

```
DECLARE @a string = "BEGIN" + @text1 + "END";
DECLARE @b string = string.Format("BEGIN{0}END", @text1);
DECLARE @c string = MyHelper.GetMyName();
```


