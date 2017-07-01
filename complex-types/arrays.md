# ARRAYs

## Overview

* An ordered list of values (all of the same type)
* Immutable
* Single-Dimension

## Sample data

```
@cities = 
    SELECT * 
      FROM 
        ( VALUES
          ( "Vermont",       "Burlington;Essex;South Burlington;Colchester;Rutland" ),
          ( "Virginia",      "Virginia Beach;Norfolk;Chesapeake;Richmond;Newport News" ),
          ( "Washington",    "Seattle;Spokane;Tacoma;Vancouver;Bellevue" ),
          ( "West Virginia", "Charleston;Huntington;Parkersburg;Morgantown;Wheeling" ),
          ( "Wisconsin",     "Milwaukee;Madison;Green Bay;Kenosha;Racine" ),
          ( "Wyoming",       "Cheyenne;Casper;Laramie;Gillette;Rock Springs" ) 
       )
    AS T(State, Cities);
```


## Creating ARRAYs

### Creating Empty arrays

```
DECLARE @m = new SqlArray<string> ( );
```
or

```
DECLARE @m = new SqlArray<string> { };
```

### Creating arrays initialized with data

```
DECLARE @m = new SqlArray<string> { "A", "B" };
```

