# User-Defined Aggregators \(UDAggs\)

UDAggs allow you to define your own aggregate functions for U-SQL for use with GROUP BY.

## Creating an aggregator

Here is a simple example of a UDAgg that manually implements a custom version of Sum

```
using Microsoft.Analytics.Interfaces;

namespace MVA_UDAgg
{
    public class MySum : IAggregate<int, long>
    {
        long total;
        public override void Init() { total = 0; }
        public override void Accumulate(int value) { total += value; }
        public override long Terminate() { return total; }
    }
}
```

## Using the UDAgg

In the SELECT clause use the AGG&lt;T&gt; operator where T is the name of your UDAgg;

```
AGG<UDAgg>(InputCol) AS OutputCOl
```

Here's a full script:

```
REFERENCE ASSEMBLY AssemblyContainingUDagg;

@t = 
  SELECT * FROM 
    (VALUES
      ("2016/03/31","1:00","mrys","@saveenr great demo yesterday", 7 ),
      ("2016/03/31","7:00","saveenr","@mrys Thanks! U-SQL RuL3Z!", 4 )
    ) AS D( date, time, author, tweet , retweets);

@results = 
    SELECT
        AGG<MVA_UDAgg.MySum>(retweets) AS totalretweets
    FROM @t
    GROUP BY date;

OUTPUT @results
    TO "/output.csv"
    USING Outputters.Csv();
```

## Recursive Aggregators

If the operation in your is associative \(https://en.wikipedia.org/wiki/Associative\_property\) then you should mark your aggregator with an attribute to indicate that the UDAgg is "recursive" \(a.k.a "associative" \). This will improve the performance substantially of your U-SQL script has to aggregate a lot of data with your UDAgg because the UDAgg can be parallelized.



The snippet below shows how to do this.

```
[SqlUserDefinedReducer(IsRecursive = true)]
public class MySum : IAggregate<int, long>
{
   ...
}
    
```

**Do not just blindly add the IsRecursive property to your UDAggs. Make sure the UDAggs support the associative property.**



