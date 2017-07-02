# Sample data

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


