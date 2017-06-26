## Recursive Aggregators

If the operation in your is associative \(https://en.wikipedia.org/wiki/Associative\_property\) then you should mark your aggregator with an attribute to indicate that the UDAgg is "recursive" \(a.k.a "associative" \). This will improve the performance substantially of your U-SQL script has to aggregate a lot of data with your UDAgg because the UDAgg can be parallelized.


The snippet below shows how to do this.

```
[SqlUserDefinedReducer(IsRecursive = true)]
public class MySum : IAggregate<int, long>
{
// your code here
}
```

**Do not just blindly add the `IsRecursive` property to your UDAggs. Make sure the UDAggs support the associative property.**



