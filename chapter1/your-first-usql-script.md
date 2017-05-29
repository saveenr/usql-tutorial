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







