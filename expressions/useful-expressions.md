# Useful expressions

If you are a C# developer, the expressions below will seem obvious. If you have a SQL background or come from another language, please read this section. It will save you a lot of time later.


### Case-sensitive string comparison

```
stringcol1 == stringcol2
```

### Case-insensitive string comparison

```
string.Equals(
    strincol1, 
    strincol1, 
    System.Text.StringComparison.CurrentCultureIgnoreCase )
```

### String starts with text

```
stringcol1.StartsWith( "prefix" )
```

### String end with text

```
stringcol1.EndsWith( "prefix" )
```

### String contains text

```
stringcol1.Contains("prefix" )
```

### Get a substring: every character after the second one

```
stringcol1.Substring(2, )
```

### Get a substring: every 5 characters after the second one

```
stringcol1.Substring(2, 5)
```

### if null, pick a default value, otherwise use the value

```
stringcol1 ?? "defval"
```

The `??` is the C# null coalescing operator.

### if an expression is true, pick one value, otherwise pick a different one

```
<expr> ? "was_true" : "was_false"
```































