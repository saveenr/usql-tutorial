# ARRAYs

## Overview

* An ordered list of values (all of the same type)
* Immutable
* Single-Dimension

## Sample data




## Creating ARRAYs

### Creating Empty arrays

```
new SqlArray<string> ( )
```

or

```
new SqlArray<string> { }
```

### Creating arrays initialized with data

```
new SqlArray<string> { "A", "B" }
```

For example:

```
@output =
    SELECT * 
    FROM
    ( VALUES
        ( "West Virginia", 
            new SqlArray<string> { 
                "Charleston", "Huntington", "Parkersburg", "Morgantown", "Wheeling" 
        )
    )
AS T(State, Cities);
```

### Creating an ARRAY from an IEnumerable


```
DECLARE @letters = new [] { "A", "B" };

@output = 
    SELECT         
        State,
        SqlArray.Create( @letters ) AS Cities
    FROM @cities;
```

| State | Cities |
| --- | --- |
| Vermont | SqlArray<string>{ "A", "B" } |
| Virginia | SqlArray<string>{ "A", "B" } |
| Washington | SqlArray<string>{ "A", "B" } |
| West Virginia | SqlArray<string>{ "A", "B" } |
| Wisconsin | SqlArray<string>{ "A", "B" } |
| Wyoming | SqlArray<string>{ "A", "B" } |


```
@output = 
    SELECT         
        State,
        SqlArray.Create( Cities.Split(';') ) AS Cities
    FROM @cities;
```

| State | Cities |
| --- | --- |
| Vermont | SqlArray<string>{ "Burlington", "Essex", "South Burlington", "Colchester", "Rutland" } |
| Virginia | SqlArray<string>{ "Virginia Beach", "Norfolk", "Chesapeake", "Richmond", "Newport News" } |
| Washington | SqlArray<string>{ "Seattle", "Spokane", "Tacoma", "Vancouver", "Bellevue" } |
| West Virginia | SqlArray<string>{ "Charleston", "Huntington", "Parkersburg", "Morgantown", "Wheeling" } |
| Wisconsin | SqlArray<string>{ "Milwaukee", "Madison", "Green Bay", "Kenosha", "Racine" } |
| Wyoming | SqlArray<string>{ "Cheyenne", "Casper", "Laramie", "Gillette", "Rock Springs" } |


## Array Indexing 

Use the array indexing operator [n] where n is a long. The first index is 0 – just like .NET

```
@output = 
    SELECT         
        State,
        SqlArray.Create( Cities.Split(';') ) AS Cities
    FROM @cities;


@output =
    SELECT
        State,
        Cities[0] AS FirstCity
    FROM @output;
```


| State | FirstCity |
| --- | --- |
| Vermont | Burlington |
| Virginia | Virginia Beach |
| Washington | Seattle |
| West Virginia | Charleston |
| Wisconsin | Milwaukee |
| Wyoming | Cheyenne |


## Removing members

```
@output = 
    SELECT         
        State,
        SqlArray.Create( Cities.Split(';') ) AS Cities
    FROM @cities;

@output =
    SELECT
        State ,
        SqlArray.Create( Cities.Where( c=>c.StartsWith("C") ) ) AS Cities
    FROM @output;
```

| State | Cities |
| --- | --- |
| Vermont | SqlArray<string>{ "Colchester" } |
| Virginia | SqlArray<string>{ "Chesapeake" } |
| Washington | SqlArray<string>{  } |
| West Virginia | SqlArray<string>{ "Charleston" } |
| Wisconsin | SqlArray<string>{  } |
| Wyoming | SqlArray<string>{ "Cheyenne", "Casper" } |


## Counting members

```
@output = 
    SELECT         
        State,
        SqlArray.Create( Cities.Split(';') ) AS Cities
    FROM @cities;

@output =
    SELECT
        State ,
        SqlArray.Create( Cities.Where( c=>c.StartsWith("C") ) ) AS Cities
    FROM @output;

@output =
    SELECT
        State ,
        Cities ,
        Cities.Count AS NumCities
    FROM @output;
```


| State | Cities | NumCities |
| --- | --- | --- |
| Vermont | SqlArray<string>{ "Colchester" } | 1 |
| Virginia | SqlArray<string>{ "Chesapeake" } | 1 |
| Washington | SqlArray<string>{  } | 0 |
| West Virginia | SqlArray<string>{ "Charleston" } | 1 |
| Wisconsin | SqlArray<string>{  } | 0 |
| Wyoming | SqlArray<string>{ "Cheyenne", "Casper" } | 2 |

