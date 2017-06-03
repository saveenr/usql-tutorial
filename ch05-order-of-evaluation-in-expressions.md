# Order of Evaluation in Expressions

There's a common pattern C# developers are used to, as shown below:

```
if ((QueryString!=null) && (QueryString.StartsWith("bing"))
{
    // do something
}
```

This pattern depends on a C# behavior (common to many languages) called "short-circuiting." Simply put, in the above example, when the code runs there's no logical reason to check both conditions if the first one returns false. Short circuiting is useful because evaluating each condition may be expensive. Thus, it is a technique that compilers use to improve the performance of C# code.

When trying to do the same thing in U-SQL there are two paths you can pick. Both are valid expressions, but one will cause problems that may not be obvious at first.

The Right Choice: Use && to keep the desired short-circuiting behavior

```
@rs1 = 
    SELECT * 
    FROM @data
    WHERE ((Name!=null) && (Name.StartsWith("bing"));
```

The Wrong Choice: Use AND which does NOT match the short-circuiting behavior.

```
@rs1 = 
    SELECT * 
    FROM @data
    WHERE ((Name!=null) AND (Name.StartsWith("bing"));
```

The second translation that uses AND will sometimes fail saying that a NullReferenceException has occurred. \(sometimes = it might work on your local box but might fail in the cluster\)

The reason is simple and by-design: with AND/OR U-SQL  will try to perform certain optimizations that result in better performance - for example it may evaluate the second part of the expression first because it assumes that there is no relationship between the two conditions.

This is a standard optimization technique and the same thing is done in many systems such as SQL. The gain this optimization provides in performance is well worth the occasional confusion it causes for new U-SQL users - so this behavior will never change.  
Summary: if you need this short-circuiting behavior use && and `||`.  
As an alternative you can use the SQL-like ALL/ANY operators which are equivalent to && and ||.

You CANNOT circumvent the order of evaluation by using multiple U-SQL statements

Of course, then you'll be tempted to write your script by splitting apart the expression as shown below.

```
@rs1 = 
    SELECT * 
    FROM @data
    WHERE Name != null;

@rs2 =
    SELECT *
    FROM @rs1
    WHERE Name.StartsWith("bing");
```

The assumption here is that the first statement executes, before the second. This assumption is wrong.

This won't work either. U-SQL is declarative language not an imperative one. Just because @rs1 is defined earlier than @rs2 in the script above it does NOT imply that the WHERE condition in @rs1 is evaluated before the WHERE in @rs2. U-SQL reserves the right to combine multiple statements together and perform optimizations. You MUST use the && operator if you want to perform short-circuiting.

