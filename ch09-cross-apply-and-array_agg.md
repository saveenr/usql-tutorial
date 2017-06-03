

## Breaking Rows Apart with CROSS APPLY

Let's examine the search log again.

```
@output =  
  SELECT  
    Region,  
    Urls  
  FROM @searchlog;
```

The query above returns something like this:

| Region | Urls |
| :--- | :--- |
| en-us | A;B;C |
| en-gb | D;E;F |

The**Urls**column contains strings, but each string is a semicolon-separated list of URLs. What happens if we want to break apart the**Urls**field so that only a URL is present on every row? For example, below is what we want to see:

| Region | Urls |
| :--- | :--- |
| en-us | A |
| en-us | B |
| en-us | C |
| en-gb | D |
| en-gb | E |
| en-gb | F |

This is a perfect job for the**CROSS APPLY**operator.

```
@output =  
  SELECT  
    Region,  
    Urls  
  FROM @searchlog;


@output =  
  SELECT  
    Region,  
    SqlArray.Create(Urls.Split(';')) AS UrlTokens  
  FROM @output;


@output =  
  SELECT  
    Region,  
    Token AS Url  
 FROM @output  
  CROSS APPLY EXPLODE (UrlTokens) AS r(Token);
```

# Putting Rows Together with ARRAY\_AGG

The**LIST**aggregate operator performs the opposite of**CROSS APPLY**.

For example, if we start with this:

| Region | Result |
| :--- | :--- |
| en-us | A |
| en-us | B |
| en-us | C |
| en-gb | D |
| en-gb | E |
| en-gb | F |

But we want this as the output:

| Region | Urls |
| :--- | :--- |
| en-us | A;B;C |
| en-gb | D;E;F |

This is exactly what the**ARRAY\_AGG**operator does. In the example below you will see rowset taken apart by**CROSS APPLY**and then reconstructed via the**ARRAY\_AGG**operator.

```
@a = SELECT Region, Urls FROM @searchlog;  


@b =  
  SELECT  
    Region,  
    SqlArray.Create(Urls.Split(';')) AS UrlTokens  
  FROM @a;


@c =  
  SELECT  
    Region,  
    Token AS Url  
  FROM @b   
   CROSS APPLY EXPLODE (UrlTokens) AS r(Token);


@d =  
  SELECT 
    Region,  
    string.Join(";", ARRAY_AGG
&
lt;string
&
gt;(Url).ToArray()) AS Urls  
  FROM @a
  GROUP BY Region;
```

| Region | Urls |
| :--- | :--- |
| en-us | A;B;C |
| en-gb | D;E;F |

| Region | Urls |
| :--- | :--- |
| en-us | SqlArray&lt;string&gt;{“A”,”B”,”C”} |
| en-gb | SqlArray&lt;string&gt;{“D”,”E”,”F”} |

| Region | Result |
| :--- | :--- |
| en-us | A |
| en-us | B |
| en-us | C |
| en-gb | D |
| en-gb | E |
| en-gb | F |

| Region | Urls |
| :--- | :--- |
| en-us | A;B;C |
| en-gb | D;E;F |

# 



