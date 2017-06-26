# Built-in Extractors

U-SQL has three built-in extractors that handle text

* `Extractors.Csv()` reads comma-separated value (CSV)
* `Extractors.Tsv()` reads tab-separated value (TSV) 
* `Extractors.Text()` reads delimited text files.

`Extractors.Csv` and `Extractors.Tsv` are the same as `Extractors.Text` but they default to a specific delimiter appropriate for the format they support.
