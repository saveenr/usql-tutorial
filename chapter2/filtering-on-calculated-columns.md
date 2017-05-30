// SYNTAX ERROR

@output =

 SELECT Start, Region,Duration/60.0 ASDurationInMinutes

 FROM @searchlog

 WHEREDurationInMinutes&gt;= 20;



// SYNTAX ERROR

output =

 SELECT Start, Region,Duration/60.0 ASDurationInMinutes

 FROM @searchlog

HAVINGDurationInMinutes&gt;= 20;







// THIS WORKS: Refine the rowset



@output =

 SELECT Start,

 Region,

Duration/60.0 ASDurationInMinutes

 FROM @searchlog;



@output =

 SELECT \*

 FROM @output

 WHEREDurationInMinutes&gt;= 20;





// THIS WORKS: Repeat the full expression



@output =

 SELECT Start,

 Region,

Duration/60.0 ASDurationInMinutes

 FROM @searchlog

 WHEREDuration/60.0&gt;= 20;









