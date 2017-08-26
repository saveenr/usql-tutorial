# CROSS APPLY and ARRAY_AGG

**CROSS APPLY** and **ARRAY_AGG** support two very common scenarios in transforming text

* CROSS APPLY can break a row apart into multiple rows
* ARRAY_AGG joins rows into a single row

What we will how is how to move back and forth between the following two RowSets


The first RowSet has a column called Urls we want to split

| Region | Urls |
| :--- | :--- |
| en-us | A;B;C |
| en-gb | D;E;F |

The second RowSet has a column called `Url` we want to merge

| Region | Url |
| :--- | :--- |
| en-us | A |
| en-us | B |
| en-us | C |
| en-gb | D |
| en-gb | E |
| en-gb | F |



## Breaking apart rows with CROSS APPLY

Let's examine the searchlog again and extract the `Region` and `Urls` columns.

```
@a = 
  SELECT 
    Region, 
    Urls
  FROM @searchlog;  
```
@a looks like this:

| Region | Urls |
| :--- | :--- |
| en-us | A;B;C |
| en-gb | D;E;F |

The **Urls** column contains strings, but each string is a semicolon-separated list of URLs.
We will eventually use CROSS APPLY to break that column apart. But first we must transform the string into an array that CROSS APPLY can work with.

```
@b =  
  SELECT 
    Region, 
    SqlArray.Create(Urls.Split(';')) AS UrlTokens  
  FROM @a;
```
@b looks like this

| Region | Urls |
| :--- | :--- |
| en-us | SqlArray<string>{"A","B","C"} |
| en-gb | SqlArray<string>{"D","E","F"} |

Now we can use CROSS APPLY to break the rows apart.

```
@c =  
  SELECT 
    Region, 
    Token AS Url  
  FROM @b   
   CROSS APPLY EXPLODE (UrlTokens) AS r(Token);
```

@c looks like this

| Region | Url |
| :--- | :--- |
| en-us | A |
| en-us | B |
| en-us | C |
| en-gb | D |
| en-gb | E |
| en-gb | F |


## Merging rows with ARRAY_AGG

Now, let's reverse the scenario and merge the Url column for each region with ARRAY_AGG

First, we'll merge the Urls together into an array with ARRAY_AGG

```
@d =  
  SELECT 
    Region, 
    ARRAY_AGG<string>(Url) AS UrlsArray  
  FROM @c
  GROUP BY Region;
```

@d looks like this

| Region | UrlsArray |
| :--- | :--- |
| en-us | SqlArray{"A","B","C"} |
| en-gb | SqlArray{"D","E","F"} |

Now that we have arrays of strings, we will collapse the each array into a string using `string.Join`.

```
@e =  
  SELECT 
    Region, 
    string.Join(";", UrlsArray) AS Urls  
  FROM @d;
```

Finally @e looks like this. We are back to where we started.


| Region | Urls |
| :--- | :--- |
| en-us | A;B;C |
| en-gb | D;E;F |


