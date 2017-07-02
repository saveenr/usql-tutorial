# String Literals


This tutorial uses different kinds of string literals. 
* The Regular C# String Literal
* The Verbatim C# String Literal – these string literals begin with a @ character

You may use either in U-SQL. The key difference are in the handling of embedded quotation marks, backslashes, and newlines as shown in the table below. 

Simple string

Rular"Foo"	@"Foo"
Quotation marks	"\"Hello\" I said"	@"""Hello"" I said"
Slashes	"a/b/c"	@"a/b/c"
Backslashes	"a\\b\\c"	@"a\b\c"
newlines	"a\r\nb\r\nc"
	@"a
b
c"


