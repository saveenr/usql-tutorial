@rows =

 EXTRACT name string, idint

 FROM

“/file1.tsv”,

 “/file2.tsv”,

 “/file3.tsv”

 USINGExtractors.Csv\( \);











