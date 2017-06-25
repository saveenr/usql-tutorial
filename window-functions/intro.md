# Window Functions

**Window functions** were introduced to the ISO/ANSI SQL Standard in 2003. U-SQL adopts a subset of window functions as defined by the ANSI SQL Standard.

Window functions are used to do computation within sets of rows called windows. Windows are defined by the OVER clause. Window functions solve some key scenarios in a highly efficient manner.

This tutorial uses two sample datasets to walk you through some sample scenario where you can apply window functions. 

The window functions are categorized into:
* Reporting aggregation functions, such as SUM or AVG
* Ranking functions, such as DENSE_RANK, ROW_NUMBER, NTILE, and RANK
* Analytic functions, such as cumulative distribution, percentiles, or accesses data from a previous row in the same result set without the use of a self-join

