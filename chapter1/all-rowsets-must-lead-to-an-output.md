There’s no syntax error here. But compilation will fail.



All rowsets must eventually contribute to data being written to a file or table.



@smallrowsdoes not meet that criteria.



@rows =  
 EXTRACT

 Name string,

 Amountint,  
 FROM “/input.csv”  
 USINGExtractors.Csv\(\);



@smallrows=

 SELECT Name, Amount

 FROM @rows

 WHERE Amount &lt; 1000;



OUTPUT @rows

 TO "/output/output.csv"

 USINGOutputters.Csv\(\);

