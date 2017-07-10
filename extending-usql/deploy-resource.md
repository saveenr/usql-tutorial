# DEPLOY RESOURCE

## Scenario

Suppose you are using a .NET library - for example one that maps IP addresses to locations. THen suppose that this .NET library loads its IP database from a file on disk - maybe a file called `ip.data`.

Now if we want to use this library in U-SQL we have to do two things: (1) make sure the .NET library is available to the script via a U-SQL assembly and (2) make sure the data file is available to the script with the `DEPLOY RESOURCE` statement.

## A simple Hello World example

The simple example below shows how it would be done with a C# expression directly in the script.


```
DEPLOY RESOURCE "/helloword.txt";

@departments =
  SELECT * 
  FROM (VALUES
      (31, "Sales"),
      (33, "Engineering"),
      (34, "Clerical"),
      (35, "Marketing")
    ) AS D( DepID, DepName );

@departments =
    SELECT DepID, DepName, System.[IO].File.ReadAllText("helloworld.txt") AS Message
    FROM @departments;

OUTPUT @departments 
    TO "/departments.tsv"
    USING Outputters.Tsv();
```
