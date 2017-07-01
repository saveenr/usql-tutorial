# Parameters

The `DECLARE` statement allows us to define parameters to store values for things that aren't RowSets.

We'll start with this snippet

```
@rows = 
  EXTRACT name string, 
           id int
  FROM "/data.csv"
  USING Extractors.Csv();
```

`DECLARE` can assign constant values to a name. In this case we can assign the input file to a parameter.

```
DECLARE @inputfile string = "/data.csv";

@rows = 
  EXTRACT 
    name string, 
    id   int
  FROM @inputfile
  USING Extractors.Csv();
```


## Parameter values cannot be assigned directly from a RowSet.

Values cannot be assigned from a RowSet to a DECLARE parameter.

```
// This does NOT work
DECLARE @maxval int = SELECT MAX(value) FROM data;
```

An alternative is to get a single-row RowSet with a single column and then JOIN that single-row RowSet other RowSet to get what you need.

