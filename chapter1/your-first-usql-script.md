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

https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/SearchLog.tsv

Take a look at the TSV file. Some things to point out

* there is no header row
* it is a tsv - tab-separated-values

Although this file is very simple, it will

