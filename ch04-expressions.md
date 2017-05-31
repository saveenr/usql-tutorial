U-SQL Expressions

Overview
Clauses such as SELECT, WHERE, and HAVING (and others) allow you to enter expressions – in particular U-SQL Expressions.
An expression in a programming language is a combination of explicit values, constants, variables, operators, and functions that are interpreted according to the particular rules of precedence and of association for a particular programming language, which computes and then produces another value.
The simplest way of thinking of a U-SQL expression is that it is a merely C# expression with some U-SQL extensions such as the AND, OR, NOT operators.
A Tip for SQL Developers
Lots of people come to U-SQL from SQL and ask how U-SQL accomplishes things they are familiar with in SQL. A great example in creating an uppercase string.
Given, what we’ve covered so far A SQL developer will expect to write the following in U-SQL
@output = 
    SELECT 
        UPPER( Region ) AS NewRegion
    FROM @searchlog;

But those developers will be disappointed to find out that U-SQL has no UPPER() method. The C# developer knows what to: just use the string type's intrinsic ToUpper() method.
@output = 
    SELECT 
        Region.ToUpper() AS NewRegion
    FROM @searchlog;

Casting types
Expressions can also be converted to a different type

@output= 
    SELECT 
        Start, 
        Region, 
         ((double) Duration) AS DurationDouble
    FROM @searchlog;



Using .NET Types
Rowset columns are strongly typed. U-SQL allows you to call methods defined on those types in the SELECT clause.
// Find what day of year each session took place 

@output= 
    SELECT 
        Start, 
        Region, 
        Start.DayOfYear AS StartDayOfYear
    FROM @searchlog; 

Creating New Objects with Constructors [NOT SUPPORTED IN USQL]
You can use standard C# expressions to create new objects
rs1 = 
    SELECT 
        Foo, 
        new MyType( Bar ) AS Beer
    FROM data;


Creating .NET Collections [DOESN’T work IN USQL]
Similar to the ability to create new objects, even collections can be created and initialized.
rs1 = 
    SELECT 
        Foo, 
        new List<int> {1,2,3} AS Beer
    FROM data;

rs2 = 
    SELECT 
        Foo, 
        new int[] {1,2,3} AS Beer
    FROM data;

rs3 = 
    SELECT 
        Foo, 
        new [] {1,2,3} AS Beer
    FROM data;




Order of Evaluation in Expressions
A Common Scenario
There's a common pattern C# developers are used to, as shown below:

if ((QueryString!=null) && (QueryString.StartsWith("bing"))
{
	// do something
}


This pattern depends on a C# behavior (common to many languages) called "short-circuiting." Simply put, in the above example, when the code runs there's no logical reason to check both conditions if the first one returns false. Short circuiting is useful because evaluating each condition may be expensive. Thus, it is a technique that compilers use to improve the performance of C# code.
When trying to do the same thing in U-SQL there are two paths you can pick. Both are valid expressions, but one will cause problems that may not be obvious at first.
The Right Choice
Use && to keep the desired short-circuiting behavior

@rs1 = 
    SELECT * 
    FROM @data
    WHERE ((Name!=null) && (Name.StartsWith("bing"));


The Wrong Choice
Use AND which does NOT match the short-circuiting behavior.

@rs1 = 
    SELECT * 
    FROM @data
    WHERE ((Name!=null) AND (Name.StartsWith("bing"));


The second translation that uses AND will sometimes fail saying that a NullReferenceException has occurred. (sometimes = it might work on your local box but might fail in the cluster)
The reason is simple and by-design: with AND/OR U-SQL  will try to perform certain optimizations that result in better performance – for example it may evaluate the second part of the expression first because it assumes that there is no relationship between the two conditions. 
This is a standard optimization technique and the same thing is done in many systems such as SQL. The gain this optimization provides in performance is well worth the occasional confusion it causes for new U-SQL users – so this behavior will never change.
Summary: if you need this short-circuiting behavior use && and ||.
As an alternative you can use the SQL-like ALL/ANY operators which are equivalent to &&/||.
You CANNOT circumvent the order of evaluation by using multiple U-SQL statements
Of course, then you'll be tempted to write your script by splitting apart the expression as shown below.
@rs1 = 
    SELECT * 
    FROM @data
    WHERE Name!=null;

@rs2 =
    SELECT *
    FROM @rs1
    WHERE Name.StartsWith("bing");

The assumption here is 
First I'll get the non-null objects and then I can avoid the null reference issue.
This won’t work either. U-SQL is declarative language not an imperative one. Just because rs1 is defined earlier than rs2 in the script above it does NOT imply that the WHERE condition in rs1 is evaluated before the WHERE in rs2. U-SQL reserves the right to combine multiple statements together and perform optimizations. You MUST use the && operator if you want to perform short-circuiting.
