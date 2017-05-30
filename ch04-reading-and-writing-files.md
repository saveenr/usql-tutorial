@rows =  
 EXTRACT

 Name string,

 Idint,  
 FROM “/input.csv”  
 USINGExtractors.Csv\(skipFirstNRows:1\);



OUTPUT @rows

 TO "/output/output.csv"

 USINGOutputters.Csv\(outputHeader:true\);


# Multiple inputs for EXTRACT

@rows =

 EXTRACT name string, idint

 FROM

“/file1.tsv”,

 “/file2.tsv”,

 “/file3.tsv”

 USINGExtractors.Csv\( \);








