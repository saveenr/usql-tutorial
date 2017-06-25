# Window Ranking Functions

Ranking functions return a ranking value (a long) for each row in each partition as defined by the PARTITION BY and OVER clauses. The ordering of the rank is controlled by the ORDER BY in the OVER clause.

The following are supported ranking functions:

- RANK
- DENSE_RANK
- ROW_NUMBER
- NTILE

Syntax:


```
[RANK() | DENSE_RANK() | ROW_NUMBER() | NTILE(<numgroups>)]
    OVER (
    [PARTITION BY <identifier, > …[n]]
    [ORDER BY <identifier, > …[n] [ASC|DESC]]
    ) AS <alias>
```

The ORDER BY clause is optional for ranking functions. If ORDER BY is specified then it determines the order of the ranking. If ORDER BY is not specified then U-SQL assigns values based on the order it reads record. Thus resulting into non deterministic value of row number, rank or dense rank in the case were order by clause is not specified.

NTILE requires an expression that evaluates to a positive integer. This number specifies the number of groups into which each partition must be divided. This identifier is used only with the NTILE ranking function.

For more details on the OVER clause, see U-SQL reference.
