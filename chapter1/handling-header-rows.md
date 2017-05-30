@rows =  
 EXTRACT

 Name string,

 Idint,  
 FROM “/input.csv”  
 USINGExtractors.Csv\(skipFirstNRows:1\);



OUTPUT @rows

 TO "/output/output.csv"

 USINGOutputters.Csv\(outputHeader:true\);

