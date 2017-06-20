Launch Visual Studio. Go to **File > New > Project > Installed > Templates > Azure Data Lake > U-SQL Project**.

At this point you should notice that an empty U-SQL script has been created called "Script.usql".

## Script Files

![](/assets/empty_usql_script_buttons.png)

The **Submit** button will run the script. The dropdown next to the **Submit** button says `(Local)`. This means that Submit will run the script in Local Execution mode. In future chapters, we'll pick an Azure Data Lake Analytics account and run the script in Cloud Execution mode - but for now we will continue to locally execute all our U-SQL script.

You can ignore the dropdowns that say `master` and `dbo`. They will not impact the tutorial in any way.

## U-SQL Projects and Code-Behind files

Make sure the Solution Explorer is visible. If it isn't go to **View &gt; Solution Explorer**.

![](/assets/solution_explorer_new_usql_proj.png)

The solution contains single U-SQL project called `USQLApplicationX`. Inside that project is the empty `Script.usql`file.

Notice that you can expand the .usql file node. It contains a C\# file. This is called the **U-SQL Code-Behind** file. You will find this code-behind file very useful in later chapters.

NOTE: U-SQL Code-behind is a convenience feature of ADLToolsForVS and not a part of the U-SQL language itself.

## Your First Compiler Error

Learning a programming language means learning how the compiler tells you something is wrong.

Press **Submit** to run your empty script in Local Execution mode. You should see the Error List window appear and a single error. Unsurprisingly, it complains of an empty script.

![](/assets/e_csc_user_generic_invalid_empty_content.png)

It's worth noticing a few things about this error.

* `CSC` : this refers to the U-SQL compiler. 
* `USER`: this means that the user \(you\) are the source of the problem. If it says `SYSTEM` it means something is wrong with the U-SQL compiler itself or perhaps the service. 

System errors are not very common - but it will be valuable later on to always understand the distinction between **user errors** \(also called **user code errors**\) and system errors.

## Inputs and Outputs

All U-SQL scripts transform inputs to outputs. There are different kinds of inputs and outputs but ultimately they resolve to one of two things:

* Files
* U-SQL tables

REMEMBER: compiling a U-SQL script requires that all the inputs must exist. We often describe this as "all inputs must be known at compile time". Later chapters will talk about this topic in more detail.

## Location of inputs and Outputs

During U-SQL Cloud Execution the inputs/outputs must all be in the cloud - typically this means Azure Data Lake Store.

During U-SQL Local Execution the inputs/outputs must all be on your own box. There's a special name for this location: The U-**SQL Local Data Root**.

You can find the local data root by going to **Tools > Data Lake > Options and Settings**. It in the field called **DataRoot **at the top.

![](/assets/vs_tools_datalake_options.png)

Copy that location and open it in Windows Explorer.

Download the `SearchLog.tsv` file into the Local Data Root from here:

[https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/SearchLog.tsv](https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/SearchLog.tsv)

Take a look at the TSV file. Some things to point out

* It is a tsv - tab-separated-value
* It has no header row
* Each row has the same number of columns

Now paste the following script in to the `Script.usql` window.

```
@searchlog = 
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    FROM "/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

This script we've just run is very simple - all it does is copy a file. However, there it introduces many interesting topics we will discuss.

Click **Submit**. 

Visual Studio will show the Job Graph window and a Console window will open. The script should run successfully. 

After the script completes, look in the Local Run Data Root folder, you should see a file called `SearchLog_output.tsv`.

**Congratulations, you've run your first U-SQL script!**
  
## Some things to notice

**U-SQL keywords are case-sensitive**

* The script contains a number of U-SQL keywords: `EXTRACT`, `FROM`, `TO`, `OUTPUT`, `USING`, etc.
* U-SQL keywords are case sensitive. Keep this in mind - it's one of the most common errors people run into.

**Reading and Writing Files with EXTRACT and OUTPUT**

* The `EXTRACT` statement reads from files. The built-in extractor called `Extractors.Tsv` handles Tab-Separated-Value files.
* The `OUTPUT` statement writes to files. The built-in outputter called `Outputters.Tsv` handles Tab-Separated-Value files.

We'll cover reading and writing to U-SQL tables in later chapters. Later, we'll also learn how to make custom extractors.

**Schema for files and Header Rows**

* From the U-SQL perspective files are "blobs" - they don't contain any usable schema information. So U-SQL supports a concept called "schema on read" - this means the developer specified the schema that is expected in the file. As you can see the names of the columns and the datatypes are specified in the `EXTRACT` statement.
* The default Extractors and Outputters cannot infer the schema from the header row - in fact by default they assume that there is no header row \(this behavior can overriden\)

**RowSets**

* The first statement in the script defines a "RowSet" called `@searchlog`. RowSets are an abstraction that represents how rows flow through a script.
* Because it comes up so often, we should clarify one thing now: RowSets are not tables, or temporary tables, or views, etc. They imply nothing about how data will be persisted.



