
## String equality

Case-sensitive equality

```
"ABC" == "ABC"
stringcol1 == "ABC"
```

Case-insensitive equality

```
string.Equals(
    "Abc",
    "ABC",
System.Text.StringComparison.CurrentCultureIgnoreCase )
```

### String starts/ends with text

```
stringcol1.StartsWith( "prefix" )
stringcol1.EndsWith( "prefix" )
```

### String contains text

```
stringcol1.Contains("prefix" )
```

### Substrings

Get every character after the second one

```
stringcol1.Substring(2)
```

Get a string of 5 characters starting after the second character in the original string

```
stringcol1.Substring(2, 5)
```

### String length

```
stringcol.Length
```

### Merging strings

```
stringcol1 + "_foo_" + stringcol2
```

Or

```
string.Format( "{0}_foo_{1}" , stringcol1, stringcol2 );
```

### Remove whitespace


From the beginning and the end of the sting
```
stringcol1.Trim()
```

From the beginning

```
stringcol1.TrimStart()
```

From the end

```
stringcol1.TrimEnd()
```

### Change case


```
stringcol1.ToLower()
stringcol1.ToUpper()
```
































































