# ARRAYs

### Overview

* An ordered list of values (all of the same type)
* Immutable
* Single-Dimension only

### Creating arrays initialized with data

You can create a SQlArray using the SqlArray constructor or use the SqlArray.Create static method

The SqlArray.Create static method can also be used to create arrays.


```
@output =
    SELECT * 
    FROM
    ( VALUES
        ( "West Virginia", new SqlArray<string>( new string [] { "Charleston", "Huntington", "Parkersburg", "Morgantown", "Wheeling"} ) ),
        ( "Wisconsin", SqlArray.Create( new string [] { "Milwaukee", "Madison", "Green Bay", "Kenosha", "Racine"} ) )

    ) AS T(State, Cities);
```

### Creating Empty arrays

```
new SqlArray<string> ( )
```

or

```
new SqlArray<string> { }
```

### Creating an ARRAY from an .NET Array

```
DECLARE @wv_cities = new string [] { "Charleston", "Huntington", "Parkersburg", "Morgantown", "Wheeling"};
DECLARE @wi_cities = new string [] { "Milwaukee", "Madison", "Green Bay", "Kenosha", "Racine" };

@cities =
    SELECT * 
    FROM
    ( VALUES
        ( "West Virginia", new SqlArray<string>( @wv_cities ) ),
        ( "Wisconsin", SqlArray.Create( @wi_cities ) )

    ) AS T(State, Cities);

```



# Sample data

```
@cities =
    SELECT * 
    FROM
    ( VALUES
        ( "Vermont","Burlington,Essex,South,Burlington,Colchester,Rutland" ),
        ( "Virginia","Virginia Beach,Norfolk,Chesapeake,Richmond,Newport News" ),
        ( "Washington","Seattle,Spokane,Tacoma,Vancouver,Bellevue" ),
        ( "West Virginia", "Charleston,Huntington,Parkersburg,Morgantown,Wheeling"),
        ( "Wisconsin","Milwaukee,Madison,Green Bay,Kenosha,Racine"),
        ( "Wyoming","Cheyenne,Casper,Laramie,Gillette,Rock Springs" )
    ) AS T(State, Cities);

@cities =
    SELECT 
        State,
        SqlArray.Create( Cities.Split(',') ) AS Cities 
    FROM @cities;
```

| State string | Cities SqlArray |
| --- | --- |
| Vermont | SqlArray{ "Burlington", "Essex", "South", "Burlington", "Colchester", "Rutland" } |
| Virginia | SqlArray{ "Virginia Beach", "Norfolk", "Chesapeake", "Richmond", "Newport News" } |
| Washington | SqlArray{ "Seattle", "Spokane", "Tacoma", "Vancouver", "Bellevue" } |
| West Virginia | SqlArray{ "Charleston", "Huntington", "Parkersburg", "Morgantown", "Wheeling" } |
| Wisconsin | SqlArray{ "Milwaukee", "Madison", "Green Bay", "Kenosha", "Racine" } |
| Wyoming | SqlArray{ "Cheyenne", "Casper", "Laramie", "Gillette", "Rock Springs" } |



### Array Indexing 

Use the array indexing operator [n] where n is a long. The first index is 0 – just like .NET

```

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


### Removing members

```
@output =
    SELECT
        State ,
        SqlArray.Create( Cities.Where( c=>c.StartsWith("C") ) ) AS Cities
    FROM @output;
```

| State | Cities |
| --- | --- |
| Vermont | SqlArray{ "Colchester" } |
| Virginia | SqlArray{ "Chesapeake" } |
| Washington | SqlArray{  } |
| West Virginia | SqlArray{ "Charleston" } |
| Wisconsin | SqlArray{  } |
| Wyoming | SqlArray{ "Cheyenne", "Casper" } |


## Counting members

```

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

