# Summary

* [Introduction](README.md) ...

* 0 Meta
  * [How to read the tutorial](meta/how-to-read.md)
  * [Contributing](meta/contributing.md)
  * [Sending feedback](meta/feedback.md)
  * [Change log](meta/change-log.md)

* 1 Getting started
  * [Overview](getting-started/getting-started-intro.md)
  * [U-SQL vs SQL](getting-started/usql-vs-sql.md)
  * [Running U-SQL scripts](getting-started/running-usql-scripts.md)
  * [Preparing for the tutorial](getting-started/preparing-for-the-tutorial.md)
  * [Your first U-SQL script](getting-started/your-first-usql-script.md)
  * [Common errors](getting-started/common-errors.md)
  * [The SearchLog dataset](getting-started/searchlog-dataset.md) 

* 2 Transforming RowSets
  * [Overview](transforming-rowsets/transforming-rowsets-intro.md)
  * [Creating RowSets from RowSets](transforming-rowsets/creating-rowsets-from-rowsets.md)
  * [Creating constant RowSets](transforming-rowsets/creating-constant-rowsets.md)
  * [Refining RowSets](transforming-rowsets/refining-rowsets.md)
  * [Modifying columns with SELECT](transforming-rowsets/modifying-columns-with-select.md)
  * [Filtering RowSets with WHERE](transforming-rowsets/filtering-rowsets-with-where.md)
  * [Numbering rows](transforming-rowsets/numbering-rows.md)
  * [RowSets must lead to an output](transforming-rowsets/rowsets-must-lead-to-an-output.md)
  * [Escaping column names](transforming-rowsets/escaping-column-names.md)

* 3 DECLARE Parameters
  * [Overview](declare-parameters/declare-parameters-intro.md)
  * [Parameter type inference](declare-parameters/parameter-type-inference.md)

* 4 Data types
  * [Overview](data-types/data-types-intro.md)
  * [Nullable types](data-types/nullable-types.md)

* 5 Expressions
  * [Overview](expressions/expressions-intro.md)
  * [Order of evaluation in expressions](expressions/order-of-evaluation-in-expressions.md)
  * [Tips for SQL developers](expressions/tips-for-sql-developers.md)

* 6 Reading and writing files
  * [Overview](reading-and-writing-files/reading-and-writing-files-intro.md)
  * [FileSets](reading-and-writing-files/filesets.md)
  * [Filtering FileSets with WHERE](reading-and-writing-files/filtering-filesets-with-where.md)
  * [FileSets with dates](reading-and-writing-files/filesets-with-dates.md)
  * [Built-in Extractors](reading-and-writing-files/built-in-extractors.md)

* 7 Grouping and aggregation
  * [Overview](grouping-and-aggregation/grouping-and-aggregation-intro.md)
  * [Filtering aggregated rows](grouping-and-aggregation/filtering-aggregated-rows.md)
  * [Aggregate functions](grouping-and-aggregation/aggregate-functions.md)
  * [CROSS APPLY and ARRAY\_AGG](grouping-and-aggregation/cross-apply-and-array_agg.md)

* 8 The U-SQL catalog
  * [Overview](usql-catalog/usql-catalog-intro.md)
  * [Databases](usql-catalog/usql-databases.md)
  * [Table-valued functions \(TVFs\)](usql-catalog/usql-table-valued-functions.md)
  * [Tables](usql-catalog/usql-tables.md)
  * [Assemblies](usql-catalog/assemblies.md)
  * [Packages](usql-catalog/packages.md)

* 9 Window functions
  * [Overview](window-functions/window-functions-intro.md)
  * [Sample data](window-functions/sample-data.md)
  * [Window functions vs. GROUP BY](window-functions/window-functions-vs-group-by.md)
  * [Window aggregate functions](window-functions/window-aggregate-functions.md)
  * [Window ranking functions](window-functions/window-ranking-functions.md)
    * [Ranking: Assign unique row numbers](window-functions/ranking-assign-new-unique-row-numbers.md)
    * [Ranking: Get TOP N rows in group](window-functions/ranking-get-top-n-rows-in-group.md)
    * [Ranking: Get row with min/max value](window-functions/ranking-get-row-with-min-max-value.md)
  * [Window analytic functions](window-functions/window-analytic-functions.md)

* 10 Set operations
  * [Overview](set-operations/set-operations-intro.md)
  * [Sample data](set-operations/sample-data.md)
  * [UNION](set-operations/union.md)
  * [INTERSECT](set-operations/intersect.md)
  * [EXCEPT](set-operations/except.md)

* 11 Joins
  * [Overview](joins/joins-intro.md)
  * [Sample data](joins/sample-data.md)
  * [CROSS JOIN](joins/cross-join.md)
  * [INNER JOIN and OUTER JOIN](joins/inner-join-and-outer-join.md.md)
  * [SEMIJOIN](joins/semijoin.md)
  * [ANTISEMIJOIN](joins/antisemijoin.md)

* 12 Extending U-SQL
  * [Overview](extending-usql/extending-usql-intro.md)
  * [User-Defined Functions (UDFs)](extending-usql/user-defined-functions.md)
  * [User-Defined Types (UDTs)](extending-usql/user-defined-types.md)
  * [User-Defined Aggregators (UDAggs)](extending-usql/user-defined-aggregators.md)

