# U-SQL is not SQL

If you come from a SQL background, you’ll notice that U-SQL queries look a lot of SQL queries. Many fundamental concepts and syntactic expressions will be very familiar to those with a background in SQL.

But U-SQL, however, is a distinct language and some of the expectations you might have from the SQL world do not carry over into U-SQL.


# Running U-SQL scripts

There are two ways to run U-SQL scripts:

* You can run U-SQL scripts on your own machine. The data read and written by this script will be on you own machine. You aren't using Azure resources to do this so there is no additional cost. This method of running U-SQL scripts is called **"U-SQL Local Execution"**
* You can run U-SQL scripts in Azure in the context of a Data Lake Analytics account. The data read or written by the script will also be in Azure - typically in an Azure Data Lake Store account. You pay for any compute and storage used by the script. This is called **"U-SQL Cloud Execution"**

For most of the tutorial we will work with **U-SQL Local Execution** because we want you to learn the language first. Later on we can explore the very rich topic that is **U-SQL Cloud Execution**.

# Preparing for the tutorial

### Operating System

* Windows 7, 8, or 10 \(x64\)

* Windows 10 is recommended

* You MUST use an x64 version of Windows

### Visual Studio

* You can use Visual Studio 2015 or Visual Studio 2013

* Visual Studio 2015 is recommended

* You can use Visual Studio Community editions

* The tutorial was written using VS 2015 Community Edition

### Azure Data Lake Tools for Visual Studio \(ADLToolsForVS\)

You must install ADLToolsForVS. Install from [**here**](http://aka.ms/ADLToolsVS).

* **Verify That it Was Installed. **Go to **Tools menu. **If you see an item in that menu called **Data Lake **then you have it installed.
* **Check for Updates. **ADLToolsForVS ships on a very frequent schedule so please check for new versions often. Go to **Tools &gt; Data Lake &gt; Check for updates**.



Launch Visual Studio. Go to **File &gt; New &gt; Project &gt; Installed &gt; Templates &gt; Azure Data Lake &gt; U-SQL \(ADLA\)**

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

Learning a language means learning how the compiler tells you something is wrong.

Press **Submit** to run your empty script in Local Execution mode. You should see the Error List window appear and a single error. Unsurprisingly, it complains of an empty script.

![](/assets/e_csc_user_generic_invalid_empty_content.png)

It's worth noticing a few things about this error.

* First, the error code contains the id `CSC` : this refers to the U-SQL compiler. 
* Second, the error code contains the id `USER`: this means that the user \(you\) are the source of the problem. If it says `SYSTEM` it means something is wrong with the U-SQL compiler itself or perhaps the service. These system errors are not very common to experience - but it will be valuable later on to always understand the distinction between **user errors** \(also called **user code errors**\) and system errors.

## Inputs and Outputs

All U-SQL scripts transform inputs to outputs. There are different kinds of inputs and outputs but ultimately they resolve to one of two things:

* files
* U-SQL tables

One thing you must always remember: a compiling a U-SQL script requires that the inputs must exist. Consequently we often describe this as "all inputs must be known at compile time". Later chapters will talk about this topic in more detail.

## Location of inputs and Outputs

During U-SQL Cloud Execution the inputs/outputs must all be in the cloud - typically this means Azure Data Lake Store.

During U-SQL Local Execution the inputs/outputs must all be on your own box. There's a special name for this location: The U-**SQL Local Data Root**.

You can find the local data root by going to Tools &gt; Data Lake &gt; Options and Settings. It in the field called **DataRoot **at the top.

![](/assets/vs_tools_datalake_options.png)

Copy that location and open it in Windows Explorer.

Download the SearchLog.tsv file into the Local Data Root from here:

[https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/SearchLog.tsv](https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/SearchLog.tsv)



# The SearchLog sample datset

You previously downloaded the SearchLog data set from this location:

[https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/SearchLog.tsv](https://www.gitbook.com/book/saveenr/usql-tutorial/edit#)

The SearchLog is a small dataset that represents "sessions" that a user has when performing a search on a web search engine. Each row represents a session for a specific search query,

The `SearchLog.tsv` file doesn't contain a header row so we'll have to document the columns below:

* **UserId** – this is an integer representing an anonymized user
* **Start** – when started a session with the search engine
* **Region** – What geographical region the user is searching from
* **Query** – What the user searched for
* **Duration** – How long their search session lasted
* **Urls** – A semicolon-separated list All the URLs that were shown to the user in the session
* **ClickedUrls** – A subset of **Urls** that the user actually clicked on \(also a semicolon-separated list\)





# Common errors

Of the many ways in which a U-SQL script is not compilable, there are only a few that account for the vast majority of issues. It WILL save you time to be familiar with the issues and how they manifest themselves in error messages and in the tools.

Try running each of the sample scripts below.

## Incorrect casing on U-SQL identifiers

The script below uses `from` instead of `FROM`.

```
@searchlog = 
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    from @"/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog 
    TO @"/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

## Input does not exist

```
@searchlog = 
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    FROM "/SearchLog_does_not_exist.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

## Invalid C\# Expression

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
    USING Extractors.Tsv_xyz();

OUTPUT @searchlog 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```



Take a look at the TSV file. Some things to point out

* there is no header row
* it is a tsv - tab-separated-values

Now paste the following script in to the Script.usql

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

Click **Submit**.The script should run successfully.

Look in the Local Run Data Root folder, you should see a file called `SearchLog_output.tsv`.

This script is very simple - all it does is copy a file, but there are many things it introduces that we should discuss.

**Case-Sensitive Keywords**

The script contains a number of U-SQL keywords: `EXTRACT`, `FROM`, `TO`, `OUTPUT`, `USING`.

U-SQL keywords are case sensitive. Keep this in mind - it's one of the most common errors people run into.

**Reading and Writing Files**

The `EXTRACT` statement reads from files. The built-in extractor called `Extractors.Tsv` handles Tab-Separated-Value files.

The `OUTPUT` statement writes to files. The built-in outputter called `Outputters.Tsv` handles Tab-Separated-Value files.

We'll cover reading and writing to U-SQL tables in later chapters. Later, we'll also learn how to make custom extractors.

**Schema for files and Header Rows**

From the U-SQL perspective files are "blobs" - they don't contain any usable schema information. So U-SQL supports a concept called "schema on read" - this means the developer specified the schema that is expected in the file. As you can see the names of the columns and the datatypes are specified in the `EXTRACT` statement.

The default Extractors and Outputters cannot infer the schema from the header row - in fact by default they assume that there is no header row \(this behavior can overriden\)

**RowSets**

The first statement in the script defines a "RowSet" called `@searchlog`. RowSets are an abstraction that represents how rows flow through a script.

Because it comes up so often, we should clarify one thing now: RowSets are not tables, or temporary tables, or views, etc. They imply nothing about how data will be persisted.

