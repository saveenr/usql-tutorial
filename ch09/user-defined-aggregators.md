User-Defined Aggregators (UDAggs)

UDAggs allow you to define your own aggregate functions for U-SQL for use with GROUP BY.

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
To use the UDAgg...

```
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

