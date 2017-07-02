# String Literals

This tutorial uses different kinds of string literals.

* The Regular C\# String Literal
* The Verbatim C\# String Literal – these string literals begin with a @ character

You may use either in U-SQL. The key difference are in the handling of embedded quotation marks, backslashes, and newlines as shown in the table below.

#### Simple string

Regular:

```
"Foo"
```

Verbatim:

```
@"Foo"
```

#### Quotation marks

Regular:


```
"\"Hello\" I said"
```

Verbatim:


```
@"""Hello"" I said"
```

#### Slashes

Regular:

```
"a/b/c"
```

Verbatim:


```
@"a/b/c"  
```

####Backslashes

Regular:


```
"a\\b\\c" 
```

Verbatim:


```
@"a\b\c"  
```

#### newlines

Regular:

```
"a\r\nb\r\nc"
```

Verbatim:

  
```
@"a  
b  
c"
```
